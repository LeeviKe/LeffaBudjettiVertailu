<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();
  export let points;

  function gameRestart() {
    dispatch('restartGame');
  }
  function goToMenu() {
    dispatch('goToMenu');
  }
</script>

<div class="backdrop" />

<div class="modal">
  <div class="overlay2">
    <h2>Points: {points}</h2>
    <button on:click={gameRestart}>Play again</button>
    <button on:click={goToMenu}>Home</button>
  </div>
</div>
