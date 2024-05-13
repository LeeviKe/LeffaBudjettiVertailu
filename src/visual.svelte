<script>
  // This component handles most of the visual stuff of the main game.
  // It shows the movie posters, names, scores etc and handles the buttons.

  import { onDestroy } from 'svelte';
  import { createEventDispatcher } from 'svelte';
  import movieList from './moviesStore';
  import tmdbLogo from './images/tmdbLogo.svg';

  const dispatch = createEventDispatcher();
  let movielist = [];

  export let movie1Image = '';
  export let movie2Image = '';

  // Getting the image links.
  $: if (movielist.length > 1) {
    movie1Image = `https://image.tmdb.org/t/p/original${movielist[0].poster_path}`;
  }
  $: if (movielist.length > 1) {
    movie2Image = `https://image.tmdb.org/t/p/original${movielist[1].poster_path}`;
  }

  const unsubscribe = movieList.subscribe((array) => {
    movielist = array;
    console.log(movielist);
  });

  onDestroy(() => {
    unsubscribe();
  });

  function leftChosen() {
    dispatch('buttonClick', 'left');
  }
  function rightChosen() {
    dispatch('buttonClick', 'right');
  }
  export let buttonsShowing;
  export let showPoints;
</script>

{#if movielist.length > 1}
  <div class="images">
    <div class="overlay2"></div>
    <div class="image1">
      <button on:click={leftChosen}>
        {#if buttonsShowing}
          {#if movielist.length > 1}
            <div class="title1">
              <h2>{movielist[0].title}</h2>
              {#if showPoints}
                <h2>{movielist[0].vote_average}</h2>
              {/if}
            </div>
          {/if}
        {/if}
        <img src={movie1Image} alt="movie1 img" />
        <div class="overlay"></div>
      </button>
    </div>
    <div class="image2">
      <button on:click={rightChosen}>
        {#if buttonsShowing}
          {#if movielist.length > 1}
            <div class="title2">
              <h2>{movielist[1].title}</h2>
              {#if showPoints}
                <h2>{movielist[1].vote_average}</h2>
              {/if}
            </div>
          {/if}
        {/if}
        <img src={movie2Image} alt="movie2 img" />
        <div class="overlay"></div>
      </button>
    </div>
  </div>
{/if}

<div class="tmdbLogo">
  <img src={tmdbLogo} alt="Tmdb logo" />
</div>
