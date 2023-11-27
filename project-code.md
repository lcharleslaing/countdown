## File: lib\components\Arrow.svelte

```
<!-- src/lib/components/Arrow.svelte -->
<script>
  export let strokeWidth = 2;
  export let fillPercentage = 0;
  export let opacity = 1;
</script>

<svg
  class="arrow"
  viewBox="0 0 100 100"
  preserveAspectRatio="none"
  xmlns="http://www.w3.org/2000/svg"
  style={`opacity: ${opacity};`}
>
  <defs>
    <linearGradient id="arrowGradient" x1="0%" y1="100%" x2="0%" y2="0%">
      <stop offset="0%" style="stop-color:orange; stop-opacity:1" />
      <stop
        offset="{fillPercentage}%"
        style="stop-color:orange; stop-opacity:1"
      />
      <stop
        offset="{fillPercentage}%"
        style="stop-color:transparent; stop-opacity:0"
      />
    </linearGradient>
    <mask id="arrowMask">
      <polygon points="50,10 90,90 50,70 10,90" fill="url(#arrowGradient)" />
    </mask>
  </defs>

  <!-- Filled Polygon -->
  <polygon points="50,10 90,90 50,70 10,90" fill="url(#arrowGradient)" />

  <!-- Outlined Polygon -->
  <polygon
    points="50,10 90,90 50,70 10,90"
    fill="none"
    stroke="black"
    stroke-width={strokeWidth}
  />
</svg>

<style>
  .arrow {
    position: absolute;
    bottom: 0;
    height: 100vh;
    width: 100%;
    z-index: 10;
  }
</style>

```

## File: lib\components\BibleVerses.svelte

```
<script>
</script>

<div class="overflow-y-auto text-center font-serif my-2 px-3">
  <div class="card w-96 bg-slate-100 bg-opacity-60 p-3 mx-auto rounded-2xl">
    <!-- This is to be pulled from the database on supabase -->
    <div class="text-left font-bold">Genesis 22:18</div>
    <div class="text-justify">
      And in thy seed shall all the nations of the earth be blessed; because
      thou hast obeyed my voice.
    </div>
  </div>
</div>

```

## File: lib\components\FlyingUpNames.svelte

```
<script>
</script>

```

## File: lib\components\JerusalemClock.svelte

```
<!-- src/lib/components/HebrewClock.svelte -->
<script>
  import { onMount } from "svelte";
  import { HDate } from "@hebcal/core";
  import { DateTime, IANAZone } from "luxon";

  let currentHebrewDate;
  let currentTimeInJerusalem;

  function updateHebrewDateAndTime() {
    const today = new Date();
    const hDate = new HDate(today);
    currentHebrewDate = hDate.toString("h");

    const jerusalemZone = new IANAZone("Asia/Jerusalem");
    currentTimeInJerusalem = DateTime.now()
      .setZone(jerusalemZone)
      .toFormat("hh:mm:ss a");
  }

  onMount(() => {
    updateHebrewDateAndTime();
    const interval = setInterval(updateHebrewDateAndTime, 1000);

    return () => clearInterval(interval);
  });
</script>

<div class="hebrew-clock text-center">
  {currentHebrewDate}
  <br />
  {currentTimeInJerusalem}
</div>

<style>
  .hebrew-clock {
    font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
    font-size: 1.2em;
    color: #333;
  }
</style>

```

## File: lib\components\JerusalemClockWithCountdown.svelte

```
<script>
  import { onMount } from "svelte";
  import { HDate } from "@hebcal/core";
  import { DateTime, IANAZone, Duration } from "luxon";
  import Arrow from "./Arrow.svelte";
  import JesusIsLord from "./JesusIsLord.svelte";

  let currentHebrewDate;
  let currentTimeInJerusalem;
  let countdown;
  let countdownPercentage;

  // Define the start and target dates
  const targetHebrewYear = 5784; // Example Hebrew year
  const startFromDate = new HDate(12, "Kislev", targetHebrewYear).greg();
  const targetGregorianDate = new Date("2023-12-07T18:00:00"); // Example target Gregorian date

  function updateHebrewDateAndTime() {
    const today = new Date();
    const hDate = new HDate(today);
    currentHebrewDate = hDate.toString("h");

    const jerusalemZone = new IANAZone("Asia/Jerusalem");
    const nowInJerusalem = DateTime.now().setZone(jerusalemZone);
    currentTimeInJerusalem = nowInJerusalem.toFormat("hh:mm:ss a");

    const targetDateTime =
      DateTime.fromJSDate(targetGregorianDate).setZone(jerusalemZone);
    const startDateTime =
      DateTime.fromJSDate(startFromDate).setZone(jerusalemZone);

    if (nowInJerusalem < targetDateTime) {
      const totalDuration = targetDateTime.diff(startDateTime);
      const elapsedDuration = nowInJerusalem.diff(startDateTime);
      countdown = Duration.fromMillis(
        targetDateTime.diff(nowInJerusalem),
      ).toFormat("dd'd' hh'h' mm'm' ss's'");
      countdownPercentage = Math.min(
        (elapsedDuration / totalDuration) * 100,
        100,
      ).toFixed(2);
    } else {
      countdown = "The target date has passed";
      countdownPercentage = "100";
    }
  }

  onMount(() => {
    updateHebrewDateAndTime();
    const interval = setInterval(updateHebrewDateAndTime, 1000);

    return () => clearInterval(interval);
  });
</script>

<div
  class="flex items-center justify-center w-screen h-screen bg-cover bg-center"
  style="background-image: url('/rapture.png');"
>
  <!-- Jesus is Lord Message -->
  <div class="absolute top-5 w-full text-center z-30">
    <div class="text-white font-extrabold text-2xl">Jesus is Lord!!!</div>
  </div>
  <div class="flex flex-col items-center justify-center space-y-1 m-4 z-0">
    <div
      class="w-full xs:w-80 shadow-md text-center card bg-slate-900 bg-opacity-90 m-1 p-2 text-slate-100 font-extrabold z-20"
    >
      <h1>Jerusalem</h1>
      <div class="text-5xl">{currentHebrewDate}</div>
      <div class="text-4xl">
        {currentTimeInJerusalem}
      </div>
    </div>

    <div
      class="w-full xs:w-80 shadow-md text-center card bg-slate-900 bg-opacity-90 m-2 p-2 text-slate-100 font-extrabold z-20"
    >
      <div>Countdown to...</div>
      <h1 class="text-5xl">24 Kislev {targetHebrewYear}</h1>
      <div class="text-4xl text-slate-100">
        {countdown} ({countdownPercentage}%)
      </div>
    </div>
  </div>
  <JesusIsLord />
  <!-- Inside your Svelte file where you use the Arrow component -->
  <Arrow strokeWidth={2} fillPercentage={countdownPercentage} opacity={0.5} />
</div>

```

## File: lib\components\JesusIsLord.svelte

```
<script>
</script>

<div class="relative top-5 z-50">
  <div class="text-green-500 font-extrabold">Jesus is Lord!!!</div>
</div>

```

## File: lib\index.js

```
// place files you want to import through the `$lib` alias in this folder.

```

## File: lib\stores.js

```
import { writable } from 'svelte/store';

export const user = writable(null);

```

## File: lib\supabase.js

```
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

export const supabase = createClient(supabaseUrl, supabaseAnonKey);

```

## File: routes\+layout.svelte

```
<script>
  import "../app.pcss";
</script>

<slot />

```

## File: routes\+page.svelte

```
<script>
  import JerusalemClockWithCountdown from "../lib/components/JerusalemClockWithCountdown.svelte";

  let isFullscreen = false;

  function toggleFullscreen() {
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen().catch((e) => {
        console.error(
          `Error attempting to enable full-screen mode: ${e.message}`,
        );
      });
      isFullscreen = true;
    } else {
      if (document.exitFullscreen) {
        document.exitFullscreen();
        isFullscreen = false;
      }
    }
  }
</script>

<!-- Auth Forms -->
<div class="">
  <section>
    <JerusalemClockWithCountdown />
  </section>
  <button on:click={toggleFullscreen}>
    {isFullscreen ? "Exit Fullscreen" : "Enter Fullscreen"}
  </button>
</div>

```

