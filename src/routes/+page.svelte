<script>
  import JerusalemClock from "$lib/components/JerusalemClock.svelte";
  import LocalClock from "$lib/components/LocalClock.svelte";
  import CountdownTimer from "$lib/components/CountdownTimer.svelte";
  import TimezoneDisplay from "../lib/components/TimezoneDisplay.svelte";

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
<div
  class="flex items-center justify-center w-screen h-screen bg-cover bg-center"
  style="background-image: url('/rapture.png');"
>
  <section>
    <!-- <JerusalemClockWithCountdownnew /> -->
    <TimezoneDisplay />
    <LocalClock />
    <JerusalemClock />
    <CountdownTimer />
  </section>
</div>
