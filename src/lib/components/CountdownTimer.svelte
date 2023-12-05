<script>
  import { onMount } from "svelte";

  let countdown = calculateCountdown();

  function calculateCountdown() {
    const now = new Date();
    const nowUtc = Date.UTC(
      now.getUTCFullYear(),
      now.getUTCMonth(),
      now.getUTCDate(),
      now.getUTCHours(),
      now.getUTCMinutes(),
      now.getUTCSeconds(),
    );
    const targetUtc = Date.UTC(2023, 11, 7, 16, 35, 0); // December 7, 2023, 4:35 PM UTC (equivalent to Jerusalem time)

    const timeLeft = targetUtc - nowUtc;

    let days = Math.floor(timeLeft / (1000 * 60 * 60 * 24));
    let hours = Math.floor(
      (timeLeft % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60),
    );
    let minutes = Math.floor((timeLeft % (1000 * 60 * 60)) / (1000 * 60));
    let seconds = Math.floor((timeLeft % (1000 * 60)) / 1000);

    return { days, hours, minutes, seconds };
  }

  onMount(() => {
    const interval = setInterval(() => {
      countdown = calculateCountdown();
    }, 1000);

    return () => clearInterval(interval);
  });
</script>

<div class="card">
  <div class="mylabel">Countdown to December 7, 2023 4:35pm (Jerusalem):</div>
  <div class="countdowntimer">
    {countdown.days}d, {countdown.hours}h, {countdown.minutes}m,
    {countdown.seconds}s
  </div>
</div>

<style>
  /* Add TailwindCSS classes as needed */
</style>
