<script>
  import { onMount } from "svelte";

  let countdown = calculateCountdown();

  function calculateCountdown() {
    const now = new Date();
    const targetDate = new Date("December 7, 2023 16:35:00").toLocaleString(
      "en-US",
      { timeZone: "Asia/Jerusalem" },
    );
    const targetTime = new Date(targetDate).getTime();
    const timeLeft = targetTime - now.getTime();

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
    {countdown.days}d, {countdown.hours}h, {countdown.minutes} m,
    {countdown.seconds}s
  </div>
</div>

<style>
  /* Add TailwindCSS classes as needed */
</style>
