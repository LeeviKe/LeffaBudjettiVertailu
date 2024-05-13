<script>
  import { fade } from 'svelte/transition';
  import MainMenuModal from './mainMenuModal.svelte';
  import Game from './game.svelte';
  import AboutModal from './aboutModal.svelte';

  let mainMenuHidden = false;
  let aboutPageHidden = true;

  function hideMainMenu() {
    mainMenuHidden = true;
  }
  function goToMenu() {
    mainMenuHidden = false;
  }
  function openAboutPage() {
    aboutPageHidden = false;
  }
  function closeAboutModal() {
    aboutPageHidden = true;
  }
</script>

{#if !mainMenuHidden}
  <div transition:fade>
    <MainMenuModal
      on:MainMenuHidden={hideMainMenu}
      on:openAboutPage={openAboutPage}
    />
  </div>
{/if}
{#if !aboutPageHidden}
  <AboutModal on:closeAboutModal={closeAboutModal} />
{/if}
<Game on:goToMenu={goToMenu} {mainMenuHidden} />
