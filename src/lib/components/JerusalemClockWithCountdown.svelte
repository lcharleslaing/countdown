<script>
  import { onMount } from "svelte";
  import { DateTime } from "luxon";

  let currentTimeInJerusalem;
  let viewerLocalEquivalentOfJerusalemTargetTime;
  let countdown;

  // Define the target date and time in Jerusalem timezone
  const targetDateTimeJerusalem = DateTime.fromObject({
    year: 2023,
    month: 12,
    day: 7,
    hour: 16,
    minute: 35,
  }).setZone("Asia/Jerusalem");

  // Define the target date and time in Jerusalem timezone in UTC
  const targetDateTimeJerusalemNew = new Date("2023-12-07T16:35:00+02:00"); // Jerusalem time zone offset is +02:00

  function convertToUserLocalTimeJS() {
    // Automatically use the user's current timezone
    return new Date(targetDateTimeUTC.toISO()).toLocaleString(undefined, {
      year: "numeric",
      month: "short",
      day: "numeric",
      hour: "numeric",
      minute: "numeric",
      hour12: true,
    });
  }

  function convertToUserTimezone(dateTime) {
    return dateTime.toLocal().toFormat("MMM dd, yyyy @ hh:mm a");
  }

  function updateCountdown() {
    const now = DateTime.local();
    currentTimeInJerusalem = now
      .setZone("Asia/Jerusalem")
      .toFormat("MMM dd, yyyy @ hh:mm a");

    viewerLocalEquivalentOfJerusalemTargetTime = convertToUserTimezone(
      targetDateTimeJerusalem,
    );

    if (now < targetDateTimeJerusalem) {
      const remainingDuration = targetDateTimeJerusalem.diff(now);
      countdown = remainingDuration.toFormat("dd'd' hh'h' mm'm' ss's'");
    } else {
      countdown = "The target date has passed";
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
  <!-- Display Countdown and Equivalent Local Time -->
  <div class="flex flex-col items-center justify-center space-y-1 m-4 z-0">
    <!-- Display Current Time in Jerusalem -->
    <div
      class="w-full xs:w-80 shadow-md text-center card bg-slate-900 bg-opacity-90 m-1 p-2 text-slate-100 font-extrabold z-20"
    >
      <h1 class="text-2xl font-medium">Jerusalem Time</h1>
      <div class="text-2xl font-semibold">{currentTimeInJerusalem}</div>
    </div>
    <!-- Countdown to Target Time -->
    <div
      class="w-full xs:w-80 shadow-md text-center card bg-slate-900 bg-opacity-90 m-2 p-2 text-slate-100 font-extrabold z-20"
    >
      <div class="text-4xl">24 Kislev, 5784</div>
      <div class="text-2xl font-medium">Sundown in Jerusalem</div>
      <div class="text-sm font-thin">
        (Equivalent Date/Time in Your Local Timezone)
      </div>
      <div class="text-xl text-green-700 font-medium">
        {convertToUserLocalTimeJS()}
      </div>
      <div class="text-4xl text-green-500">{countdown}</div>
    </div>
  </div>
</div>
