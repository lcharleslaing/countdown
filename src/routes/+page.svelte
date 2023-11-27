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
