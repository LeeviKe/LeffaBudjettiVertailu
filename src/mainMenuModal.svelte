<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  function startGame() {
    dispatch('MainMenuHidden');
  }
  function openAboutPage() {
    dispatch('openAboutPage');
  }
</script>

<div class="mainMenuModal">
  <div class="overlay2"></div>
  <div class="test">
    <h2>Movie score quiz game</h2>
    <h3>Try to guess which movie has the higher viewer score</h3>
    <div class="buttons">
      <button on:click={startGame}>Play</button>
      <button on:click={openAboutPage}>About</button>
    </div>
  </div>
</div>
