<script setup>
import { getTransitionRawChildren, ref } from "vue";
import axios from "axios";
let information = ref(null);
let trailer = ref(null);
let key = "158d1cb56293f1fb163c660df9925f96";

async function grabOption() {
  const id = movies.value;
  information.value = (
    await axios.get(
      `https://api.themoviedb.org/3/movie/${id}?api_key=${key}&append_to_response=videos`
    )
  ).data;
  trailer.value = information._rawValue.videos.results.filter((trailer) => {
    return trailer.type === "Trailer";
  });
}
</script>

<template>
  <body>
    <h1>Movies for movie night</h1>
    <p>Based off of an objectively good opinon.</p>
    <label for="movies">Choose a movie:</label>
    <select id="movies">
      <option value="635302">Demon Slayer: Mugen Train</option>
      <option value="610150">Dragon Ball Super: Super Hero</option>
      <option value="503314">Dragon Ball Super: Broly</option>
      <option value="129">Spirited Away</option>
      <option value="8392">My Neighbour Totoro</option>
      <option value="900667">One Piece: Film Red</option>
      <option value="315162">Puss in Boots: The Last Wish</option>
      <option value="24428">The Avengers</option>
      <option value="11836">The SpongeBob SquarePants Movie</option>
      <option value="980078">Winnie the Pooh: Blood and Honey</option>
    </select>
    <button id="button" @click="grabOption()">Load</button>
    <h1 id="title"></h1>
    <h3 id="overview" class="clear"></h3>
    <div id="grid" v-if="information">
      <div id="storage">
        <h4 id="overview">{{ information.overview }}</h4>
        <h4 id="original-language"></h4>
        <h4 id="budget">Amount spent making this movie ($):{{ information.budget }}</h4>
        <h4 id="genre">{{ information.genre }}</h4>
        <h4 id="vote-average">Average votes: {{ information.vote_average }}</h4>
        <h4 id="vote-count">Vote count: {{ information.vote_count }}</h4>
        <h4 id="release-date">Release date:{{ information.release_date }}</h4>
        <h4 id="popularity">Popularity: {{ information.popularity }}</h4>
        <h4 id="summary">{{ information.summary }}</h4>
        <h4 id="revenue">Amount this movie earned ($): {{ information.revenue }}</h4>
        <iframe id="videos"
        :src="`https://www.youtube.com/embed/${trailer.at(0).key}`"
        allowfullscreen
        ></iframe>
        <img
        id="poster"
        :src="`https://image.tmdb.org/t/p/original/${information.poster_path}`"
        alt="Movie Poster"
        />
      </div>
    </div>
  </body>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Limelight&display=swap");
body {
  background-image: url(https://media.istockphoto.com/id/1182610296/photo/cinema-concept-popcorn-in-a-box-and-movie-clapper-on-wooden-background-top-view-free-space.jpg?b=1&s=170667a&w=0&k=20&c=GYcqo_4-B_Jn6cbmNP3uWgdGNclbwcwtV7KQHu1wzJY=);
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: 100%;
  text-align: center;
  color: rgb(202, 102, 128);
  font-family: "limelight", "Space Mono", monospace;
  height: 130vh;
  width: 100vw;
}
#poster {
  height: 50rem;
  width: 35rem;
  border-width: 0cm;
}
#Load {
  background-color: blue;
}
#grid {
  display: grid;
  grid-template-columns: auto auto;
}
#trailer {
  width: 15rem;
}
#poster {
  aspect-ratio: 9/16;
}
</style>
