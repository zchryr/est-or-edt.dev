<script>
  import { onMount } from 'svelte';

  let shortResult = '';
  let fullResult = '';

  function checkEasternTimeZone() {
    const now = new Date();
    const year = now.getFullYear();

    // Define DST start and end dates for the current year
    const dstStart = new Date(year, 2, 14 - (year % 4)); // Second Sunday in March
    const dstEnd = new Date(year, 10, 7 - (year % 4)); // First Sunday in November

    // Adjust to 2 AM
    dstStart.setHours(2, 0, 0, 0);
    dstEnd.setHours(2, 0, 0, 0);

    // Check if current date is within DST period
    if (now >= dstStart && now < dstEnd) {
      shortResult = "EDT";
      fullResult = "Eastern Daylight Time";
    } else {
      shortResult = "EST";
      fullResult = "Eastern Standard Time";
    }
  }

  onMount(() => {
    checkEasternTimeZone();
    const interval = setInterval(checkEasternTimeZone, 1000);
    return () => clearInterval(interval);
  });
</script>

<main>
  <div class="container">
    <p class="main-result">It is: <span class="short-result">{shortResult}</span></p>
    <p class="full-result">{fullResult}</p>
  </div>
</main>

<style>
  main {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f0f0f0;
  }
  .container {
    text-align: center;
    padding: 20px;
    background-color: white;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
  }
  .main-result {
    font-size: 2em;
    margin-bottom: 10px;
  }
  .short-result {
    font-weight: bold;
  }
  .full-result {
    font-size: 1.2em;
  }
</style>