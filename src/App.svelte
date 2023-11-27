<script>
  import { movies } from "./mockMovies.js";
  import { onMount } from "svelte";

  let selectedChannel = null;
  let selectedMovie = movies[Math.floor(Math.random() * movies.length)];
  let filteredMovies = [];

  const channelInfo = [
    ...new Set(
      movies.map((movie) => ({
        number: movie.channel.number,
        name: movie.channel.name,
      }))
    ),
  ];

  const selectMovie = (channelNumber) => {
    selectedChannel = channelNumber;
    filterMovies();
  };

  const filterMovies = () => {
    filteredMovies = movies.filter(
      (movie) =>
        selectedChannel === null || movie.channel.number === selectedChannel
    );
    if (filteredMovies.length > 0) {
      selectedMovie =
        filteredMovies[Math.floor(Math.random() * filteredMovies.length)];
    } else {
      selectedMovie = null;
    }
  };

  // Ensure the selectMovie function runs when the component mounts
  onMount(() => {
    selectMovie(selectedChannel);
  });
</script>

<main>
  <div class="titulo">
    <h2>TV ></h2>
    <h1>PELíCULAS</h1>
  </div>
  <div class="centered-section">
    {#if selectedMovie}
      <img
        src={`/img/poster/${selectedMovie.id}.jpg`}
        alt={`${selectedMovie.title}`}
      />

      <div class="movieDescription">
        <h2 class="movieTitle">{selectedMovie.title.toLowerCase()}</h2>
        <div class="channel">
          <img
            class="movieImage"
            src={`/img/channel/${selectedMovie.channel.number}.png`}
            alt={`${selectedMovie.channel.name}`}
          />
          <div>{selectedMovie.channel.name} {selectedMovie.channel.number}</div>
        </div>
        <div margin:10px align="justify/left/right/center">
          {selectedMovie.description}
        </div>
        <div>
          {selectedMovie.category} | {selectedMovie.clasification} | {selectedMovie.duration}
          min
        </div>
        <button>VER AHORA</button>
      </div>
    {/if}
  </div>
</main>

<footer>
  {#each channelInfo as { number, name }}
    <button on:click={() => selectMovie(number)} tabindex="0">
      <img src={`/img/channel/${number}.png`} alt={`${number}`} />
      <p>{name}</p>
      <p>{number}</p>
    </button>
  {/each}
</footer>

<style>
  @import url("https://fonts.googleapis.com/css?family=Open+Sans+Condensed:300,700");

  main {
    display: flex;
    font-family: "Open Sans Condensed", sans-serif;
    flex-direction: column;
  }

  .titulo {
    display: flex;
    font-family: "Open Sans Condensed", sans-serif;
    border: 0;
    justify-content: flex-start;
    align-items: baseline;
    font-size: larger;
    color: rgb(74, 103, 154);
  }
  div {
    display: flex;
    font-family: "Open Sans Condensed", sans-serif;
    align-items: center;
    flex-direction: row;
    justify-content: flex-end;
  }
  .centered-section {
    display: flex;
    justify-content: center;
    flex-direction: row;
    align-items: stretch;
    border: 1px solid lightgrey;
    border-radius: 3%;
    max-height: 800px;
    margin: 20px auto;
  }
  .movieImage {
    display: flex;
    border-radius: 3%;
    justify-content: center;
    margin: auto;
    order: 0;
  }

  div button {
    border-radius: 5%;
    background-color: rgb(255, 152, 0);
    font-weight: 800;
    color: white;
  }
  .channel {
    display: flex;
    align-items: flex-start;
    margin: 10px;
    flex-direction: column;
    padding: 10px;
    border: 1px solid lightgrey;
    border-radius: 5px;
  }
  .movieTitle {
    color: rgb(98, 43, 139);
    font-family: "Open Sans Condensed", sans-serif;
    font-size: 2.8em;
    font-weight: 600;
    order: 1;
  }
  .movieDescription {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-items: flex-start;
    margin: 0 auto;
    padding: 20px;
    width: 400px;
    order: 1;
  }

  .movieDescription button {
    order: 2;
  }

  footer {
    display: flex;
    flex-wrap: nowrap;
    font-family: "Open Sans Condensed", sans-serif;
    justify-content: center;
    flex-direction: row;
    background-color: rgb(242, 242, 242);
  }
  footer button {
    align-content: space-around;
    align-items: stretch;
    padding: 10px;
    background-color: white;
    margin: 10px;
    width: 200px;
    height: 190px;
    border-radius: 5%;
  }

  @media (max-width: 740px) {
    .centered-section {
      display: flex;
      margin: 0;
      padding: 0;
      flex-direction: column;
    }
    .movieDescription {
      display: flex;
    }

    footer {
      display: flex;
      flex-wrap: wrap;
      height: automatic;
    }
    .channel {
      display: flex;
      margin: 10px;
      flex-direction: column;
      border: 1px solid lightgrey;
      border-radius: 5px;
    }
    .movieTitle {
      display: flex;
      flex-direction: column;
    }
    .movieDescription div {
      order: 4;
      display: flex;
      flex-direction: column;
    }
    .movieDescription button {
      order: 5;
      align-items: flex-end;
    }
  }
</style>
