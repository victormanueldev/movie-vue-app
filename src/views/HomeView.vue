<template>
  <div class="home">
    <div class="card">
      <router-link to="/movie/tt1160419">
        <img
          src="https://posterspy.com/wp-content/uploads/2020/04/DuneFINAL-1200x609.jpg"
          alt="feature-img"
        />
        <div class="detail">
          <h3>Dune</h3>
          <p>
            Feature adaptation of Frank Herbert's science fiction novel about
            the son of a noble family entrusted with the protection of the most
            valuable asset and most vital element in the galaxy.
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
      <input type="text" placeholder="Search for a movie" v-model="search" />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster" />
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="movie-detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const SearchMovies = () => {
      if (search.value != "") {
        fetch(
          `https://www.omdbapi.com/?apikey=${process.env.VUE_APP_API}&s=${search.value}`
        )
          .then((response) => response.json())
          .then((data) => {
            movies.value = data.Search;
            search.value = "";
          });
      }
    };

    return {
      search,
      movies,
      SearchMovies,
    };
  },
};
</script>

<style lang="scss">
.home {
  .card {
    position: relative;
  }

  img {
    display: block;
    width: 100%;
    height: 300px;
    object-fit: cover;
    position: relative;
  }

  .detail {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 1rem;
    z-index: 1;
    color: #fff;

    h3 {
      margin-bottom: 1rem;
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 1rem;

    input {
      display: block;
      border: none;
      outline: none;
      background: none;
      width: 100%;
      padding: 0.8rem;
      border-radius: 8px;
      color: #fff;

      &[type="text"] {
        font-size: 16px;
        border: 1px solid white;
        margin-bottom: 1rem;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }
      }

      &[type="submit"] {
        background-color: #42b883;
        font-size: 1.2rem;
        text-transform: uppercase;
        letter-spacing: 2px;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 0.7rem 0.5rem;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: 275px;
            border: 1px solid white;
            border-bottom: none;
          }

          .type {
            position: absolute;
            bottom: 16px;
            left: 0px;
            padding: 0.7rem 0.5rem;
            text-transform: capitalize;
            background-color: #42b883;
            color: #fff;
          }
        }

        .movie-detail {
          border: 1px solid white;
          border-top: none;
          padding: 0.7rem 0.5rem;
          flex: 1 1 100%;
          border-radius: 0px 0px 6px 6px;

          .year {
            color: #aaa;
            font-size: 14px;
          }

          h3 {
            color: #fff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>
