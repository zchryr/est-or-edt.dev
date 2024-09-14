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

  // Update the document title whenever shortResult changes
  $: document.title = shortResult;
</script>

<main>
  <div class="container">
    <p class="main-result">It is: <span class="short-result">{shortResult}</span></p>
    <p class="full-result">{fullResult}</p>
  </div>
  <a href="https://github.com/zchryr/est-or-edt.dev" target="_blank" rel="noopener noreferrer" class="github-link">
    <svg height="32" aria-hidden="true" viewBox="0 0 16 16" version="1.1" width="32" data-view-component="true" class="octicon octicon-mark-github v-align-middle">
      <path fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0016 8c0-4.42-3.58-8-8-8z"></path>
    </svg>
  </a>
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
  .github-link {
    position: fixed;
    bottom: 20px;
    right: 20px;
    opacity: 0.7;
    transition: opacity 0.3s ease;
  }
  .github-link:hover {
    opacity: 1;
  }
</style>