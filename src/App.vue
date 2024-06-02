<script>
import AppHeader from './components/AppHeader.vue';
import axios from 'axios';

export default {
  name: "App",
  components: {
    AppHeader
  },
  data() {
    return {

      movies: [],
      searchMovie: "",
      imagePrefix: "https://image.tmdb.org/t/p/w780",
      bandiere: {
        it: "https://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Flag_of_Italy.svg/1200px-Flag_of_Italy.svg.png",
        en: "https://upload.wikimedia.org/wikipedia/commons/a/a5/Flag_of_the_United_Kingdom_%281-2%29.svg",
      }
    }
  },
  methods: {
    getImageUrl(imagePath) {
      return this.imagePrefix + imagePath;
    },
    getVote(vote) {
      let newVote = vote / 2;
      newVote = newVote.toFixed(2);
      newVote = Math.round(newVote);
      return newVote;
    },
    getStars(vote, numeroStella) {
      let newVote = this.getVote(vote)

      let classe = 'fa-regular fa-star';

      if (newVote >= numeroStella) {
        classe = 'fa fa-star';
      }

      return classe;
    },
    getFlag(language) {

      let result = false;

      if (language == "it") {
        result = "https://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Flag_of_Italy.svg/1200px-Flag_of_Italy.svg.png";
      } else if (language == "en") {
        result = "https://upload.wikimedia.org/wikipedia/commons/a/a5/Flag_of_the_United_Kingdom_%281-2%29.svg";
      }

      return result;
    },
    getMovie() {
      console.log("cerca:", this.searchMovie);

      const options = {
        method: 'GET',
        url: 'https://api.themoviedb.org/3/search/movie',
        params: { query: 'searchMovie', include_adult: 'false', language: 'en-US', page: '1' },
        headers: {
          accept: 'application/json',
          Authorization: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI0Y2UzY2ZmYzI3Yzk5MjAwNTA4ZmNlY2M2M2Q3MWRlMyIsInN1YiI6IjY2NTgzNGJkMWQwNGYzYjQ0ZDJkMjI5MSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.BJlehlhouwfBWBii0qcuzZtqQYUEJtjvQbnG4bE_dVU'
        }
      };

      axios
        .request(options)
        .then(function (response) {
          console.log(response.data);
        })
        .catch(function (error) {
          console.error(error);
        });
    }
  },
  mounted() {
    console.log("App montata");


  }
}
</script>

<template>
  <AppHeader />
  <input type="text" v-model="searchMovie" placeholder="Cosa vuoi cercare oggi?">
  <button @click="getMovie">Cerca!</button>

  <div class="card" v-for="film in movies">
    <p>{{ film.title }}</p>
    <img class="poster" :src="imagePrefix + film.poster_path" />
    <!-- <img class="poster" :src="getImageUrl(film.poster_path)" /> -->

    <!-- <p>{{ (film.vote_average / 2).toFixed(2) }}</p> -->
    <p>Voto: {{ getVote(film.vote_average) }}</p>
    <p>
      <!-- si può fare lo stesso con un v-for? -->
      <i :class="getStars(film.vote_average, 1)"></i>
      <i :class="getStars(film.vote_average, 2)"></i>
      <i :class="getStars(film.vote_average, 3)"></i>
      <i :class="getStars(film.vote_average, 4)"></i>
      <i :class="getStars(film.vote_average, 5)"></i>
    </p>

    <img v-if="getFlag(film.original_language)" class="flag" :src="getFlag(film.original_language)" />
    <p v-else>{{ film.original_language }}</p>

  </div>

</template>

<style>
.card {
  margin-bottom: 1rem;
}

.card .poster {
  width: 6rem;
}

.flag {
  width: 1.5rem;
}
</style>