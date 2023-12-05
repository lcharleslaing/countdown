<script>
  import { onMount } from "svelte";

  let currentDate = new Date().toLocaleDateString("en-US", {
    weekday: "long", // e.g., Monday
    year: "numeric", // e.g., 2023
    month: "long", // e.g., January
    day: "numeric", // e.g., 31
  });
  let currentTime = new Date().toLocaleTimeString();

  onMount(() => {
    const interval = setInterval(() => {
      currentDate = new Date().toLocaleDateString("en-US", {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric",
      });
      currentTime = new Date().toLocaleTimeString();
    }, 1000);

    return () => clearInterval(interval);
  });
</script>

<div class="card">
  <p class="mylabel">Local Time:</p>
  <p class="localtime">{currentDate}</p>
  <p class="localtime">{currentTime}</p>
</div>

<style>
  /* Add TailwindCSS classes as needed */
</style>
