<script>
  import { onMount } from "svelte";

  let currentDate = new Date().toLocaleDateString("en-US", {
    timeZone: "Asia/Jerusalem",
    weekday: "long", // e.g., Monday
    year: "numeric", // e.g., 2023
    month: "long", // e.g., January
    day: "numeric", // e.g., 31
  });
  let currentTime = new Date().toLocaleTimeString("en-US", {
    timeZone: "Asia/Jerusalem",
  });

  onMount(() => {
    const interval = setInterval(() => {
      currentDate = new Date().toLocaleDateString("en-US", {
        timeZone: "Asia/Jerusalem",
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric",
      });
      currentTime = new Date().toLocaleTimeString("en-US", {
        timeZone: "Asia/Jerusalem",
      });
    }, 1000);

    return () => clearInterval(interval);
  });
</script>

<div class="card">
  <p class="mylabel">Jerusalem Time:</p>
  <p class="jerusalemtime">{currentDate}</p>
  <p class="jerusalemtime">{currentTime}</p>
</div>

<style>
  /* Add TailwindCSS classes as needed */
</style>
