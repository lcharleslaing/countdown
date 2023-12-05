<script>
  import { onMount } from "svelte";
  import { DateTime, Duration } from "luxon";
  import Arrow from "./Arrow.svelte";

  let currentTimeInJerusalem;
  let viewerLocalTime;
  let countdown;
  let countdownPercentage;

  // Define the target date and time
  const targetDateTime = DateTime.local(2023, 12, 7, 16, 35).setZone(
    "Asia/Jerusalem",
  );

  // Define the start date for the countdown (when the script runs)
  const startDateTime = DateTime.now().setZone("Asia/Jerusalem");
  const totalDuration = targetDateTime.diff(startDateTime);

  function updateCountdown() {
    const nowInJerusalem = DateTime.now().setZone("Asia/Jerusalem");
    currentTimeInJerusalem = nowInJerusalem.toFormat("hh:mm:ss a");

    // Get viewer's local time
    viewerLocalTime = DateTime.local().toFormat("hh:mm:ss a");

    if (nowInJerusalem < targetDateTime) {
      const remainingDuration = targetDateTime.diff(nowInJerusalem);
      countdown = remainingDuration.toFormat("dd'd' hh'h' mm'm' ss's'");
      countdownPercentage = Math.max(
        (1 - remainingDuration.as("seconds") / totalDuration.as("seconds")) *
          100,
        0,
      ).toFixed(2);
    } else {
      countdown = "The target date has passed";
      countdownPercentage = "100";
    }
  }

  onMount(() => {
    updateCountdown();
    const interval = setInterval(updateCountdown, 1000);

    return () => clearInterval(interval);
  });
</script>

<div
  class="flex items-center justify-center w-screen h-screen bg-cover bg-center"
  style="background-image: url('/rapture.png');"
>
  <!-- Jesus is Lord Message -->
  <div class="absolute top-5 w-full text-center z-30">
    <div class="text-yellow-500 font-extrabold text-4xl">Jesus is Lord!!!</div>
  </div>
  <div class="flex flex-col items-center justify-center space-y-1 m-4 z-0">
    <div
      class="w-full xs:w-80 shadow-md text-center card bg-slate-900 bg-opacity-90 m-1 p-2 text-slate-100 font-extrabold z-20"
    >
      <h1 class="text-2xl">Your Local Time</h1>
      <div class="text-4xl">{viewerLocalTime}</div>
    </div>
    <div
      class="w-full xs:w-80 shadow-md text-center card bg-slate-900 bg-opacity-90 m-1 p-2 text-slate-100 font-extrabold z-20"
    >
      <h1 class="text-2xl">Jerusalem Time</h1>
      <div class="text-4xl">{currentTimeInJerusalem}</div>
    </div>

    <div
      class="w-full xs:w-80 shadow-md text-center card bg-slate-900 bg-opacity-90 m-2 p-2 text-slate-100 font-extrabold z-20"
    >
      <div class="text-4xl">24 Kislev, 5784</div>
      <div class="text-2xl">Sundown in Jerusalem</div>
      <div class="text-4xl text-slate-100">
        {countdown}
      </div>
    </div>
  </div>
  <!-- Arrow component -->
  <!-- <Arrow strokeWidth={2} fillPercentage={countdownPercentage} opacity={0.5} /> -->
</div>
