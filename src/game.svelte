<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
  import { fade } from 'svelte/transition';
  import { onDestroy } from 'svelte';
  import { randomMovies } from './randomMovie.svelte';
  import movieList from './moviesStore';
  import Visual from './visual.svelte';
  import GameOverModal from './gameOverModal.svelte';

  let movielist = [];
  $: movielist;
  export let buttonsShowing = false;

  let gameOver = false;
  let points = 0;
  let switchingLeft = false;
  let deta = '';
  let showPoints = false;

  const unsubscribe = movieList.subscribe((array) => {
    movielist = array;
    console.log(movielist);
  });

  // The function below runs the main game
  async function game() {
    gameOver = false;

    movieList.set([]);
    points = 0;

    await randomMovies(2, false);
    do {
      if (movielist[0].id === movielist[1].id) {
        removeMovie(1);
        randomMovies(1, false);
      }
      buttonsShowing = true;

      // The buttonClicked function makes a new promise and then waits for any click to be made before continuing.
      // Meanwhile visual.svelte dispatches a button press and gives the message to deta. (message tells which button was pressed)
      // After every single click happening in the app, the function checks if the deta got updated.
      // The solution is absolutely horrible and I hate it, but it works lmao.

      const buttonClicked = await new Promise((resolve) => {
        window.addEventListener('click', () => {
          resolve();
        });
      });

      if (deta === 'left') {
        // Checks if the chosen button was the higher score.
        if (movielist[0].vote_average >= movielist[1].vote_average) {
          points++;
          await new Promise((resolve) => {
            showPoints = true;
            setTimeout(() => {
              showPoints = false;
              resolve();
            }, 1000);
          });
          if (switchingLeft) {
            await removeMovie(0);
            await randomMovies(1, true);
            switchingLeft = false;
          } else {
            await removeMovie(1);
            await randomMovies(1, false);
            switchingLeft = true;
          }
        } else {
          gameOver = true;
        }
        console.log(points);
      }

      if (deta === 'right') {
        // Checks if the chosen button was the higher score.
        if (movielist[1].vote_average >= movielist[0].vote_average) {
          points++;
          await new Promise((resolve) => {
            showPoints = true;
            setTimeout(() => {
              showPoints = false;
              resolve();
            }, 1000);
          });
          if (switchingLeft) {
            await removeMovie(0);
            await randomMovies(1, true);
            switchingLeft = false;
          } else {
            await removeMovie(1);
            await randomMovies(1, false);
            switchingLeft = true;
          }
        } else {
          gameOver = true;
        }
        console.log(points);
      }
    } while (!gameOver);
    console.error('--- GAME OVER ---');
    console.error(`Points: ${points}`);
    deta = '';
  }
  // This removes a movie from the movieList
  function removeMovie(place) {
    movieList.update((array) => {
      array.splice(place, 1);
      return array;
    });
  }

  // game();

  onDestroy(() => {
    unsubscribe();
  });

  function cliked(event) {
    deta = event.detail;
  }
  function restartGame() {
    game();
  }
  function goToMenu() {
    movieList.set([]);
    gameOver = false;
    dispatch('goToMenu');
  }
  $: {
    if (mainMenuHidden) {
      game();
    }
  }

  export let mainMenuHidden = true;
</script>

<Visual {buttonsShowing} {showPoints} on:buttonClick={cliked} />

{#if gameOver}
  <div transition:fade={{ duration: 200 }}>
    <GameOverModal
      {points}
      on:restartGame={restartGame}
      on:goToMenu={goToMenu}
    />
  </div>
{/if}
