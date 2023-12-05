<script>
  import { onMount, createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  let currentTimezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
  let customTimezone = currentTimezone;

  function updateTimezone(event) {
    customTimezone = event.target.value;
    dispatch("timezoneChange", { timezone: customTimezone });
  }

  onMount(() => {
    dispatch("timezoneChange", { timezone: currentTimezone });
  });
</script>

<div class="card">
  <div class="mylabel">Your Current Timezone:</div>
  <div class="timezone">{currentTimezone}</div>
</div>

<style>
  /* Add TailwindCSS classes as needed */
</style>
