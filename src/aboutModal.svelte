<!-- This component has a quick input validation for user's names and TMDB's demanding attribution. -->

<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  function closeAboutModal() {
    dispatch('closeAboutModal');
  }

  let firstNameValue = '';
  let lastNameValue = '';

  let firstNameValueValid = false;
  let lastNameValueValid = false;
  let firstNameInputVisited = false;
  let lastNameInputVisited = false;

  $: if (firstNameValue.trim().length > 0) {
    firstNameValueValid = true;
  } else {
    firstNameValueValid = false;
  }

  $: if (firstNameValue.trim().length > 0) {
    lastNameValueValid = true;
  } else {
    lastNameValueValid = false;
  }

  function firstNameInputVisit() {
    firstNameInputVisited = true;
  }
  function lastNameInputVisit() {
    lastNameInputVisited = true;
  }
</script>

<div class="mainMenuModal">
  <div class="overlay2"></div>
  <div class="test">
    <h2>Profile</h2>
    <h3>First name</h3>
    <input
      type="text"
      bind:value={firstNameValue}
      on:blur={firstNameInputVisit}
    />
    {#if !firstNameValueValid && firstNameInputVisited}
      <p>Input is not valid!</p>
    {/if}

    <h3>Last name</h3>
    <input
      type="text"
      bind:value={lastNameValue}
      on:blur={lastNameInputVisit}
    />

    {#if !lastNameValueValid && lastNameInputVisited}
      <p>Input is not valid!</p>
    {/if}
    <button on:click={closeAboutModal}>Home</button>
    <div class="tmbdAttribution">
      <h4>
        This application uses TMDB and the TMDB APIs but is not endorsed,
        certified, or otherwise approved by TMDB.
      </h4>
    </div>
  </div>
</div>
