<!-- The purpose of this component is to get data about different movies from different TMDB APIs, -->
<!-- and then send this data to the moviesStore for further use. -->

<script context="module">
  import movieList from './moviesStore';

  const api_key = import.meta.env.VITE_TMBD_API_KEY;
  let movielist = [];

  movieList.subscribe((array) => {
    movielist = array;
  });

  // The function below randomly picks the amount of movies given in the parameter and sends the movies to the MoviesStore
  // The seconds parameter tells if the picked movie is meant to put to the start or to the end of the movielist.
  export async function randomMovies(amountOfMovies, leftSide) {
    for (let x = 1; x <= amountOfMovies; x++) {
      const which = Math.floor(Math.random() * 4) + 1;
      if (which !== 1) {
        // 3/4 change that this activates (top rated list)
        const response = await fetch(
          `https://api.themoviedb.org/3/movie/top_rated?api_key=${api_key}&page=${Math.floor(Math.random() * 10) + 1}`
        );
        if (!response.ok) {
          throw new Error('Cannot fetch the movie data');
        }
        let movies = await response.json();
        let chosenMovie = movies.results[Math.floor(Math.random() * 20)];
        if (leftSide) {
          movieList.update((movie) => [chosenMovie, ...movie]);
        } else {
          movieList.update((movie) => [...movie, chosenMovie]);
        }
      } else {
        // 1/4 change that this activates (now playing list)
        const response = await fetch(
          `https://api.themoviedb.org/3/movie/now_playing?api_key=${api_key}&page=${Math.floor(Math.random() * 2) + 1}`
        );
        if (!response.ok) {
          throw new Error('Cannot fetch the movie data');
        }
        let movies = await response.json();
        let chosenMovie = movies.results[Math.floor(Math.random() * 20)];
        if (leftSide) {
          movieList.update((movie) => [chosenMovie, ...movie]);
        } else {
          movieList.update((movie) => [...movie, chosenMovie]);
        }
      }
    }
  }
</script>
