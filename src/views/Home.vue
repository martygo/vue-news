<template>
  <div class="home">
    <div class="feature-card">
      <a
        href="https://coronavirus.saude.gov.br/sobre-a-doenca#o-que-e-covid"
        target="_blank"
      >
        <img
          src="../assets/cdc-w9KEokhajKw-unsplash.jpg"
          class="featured-img"
        />
        <div class="detail">
          <h3>Coronavírus</h3>
          <p>
            Lorem ipsum, dolor sit amet consectetur adipisicing elit. Eveniet
            pariatur voluptas labore, tenetur repellendus tempora assumenda in
            quibusdam ipsam laboriosam obcaecati iure architecto voluptatem quae
            numquam nemo maxime unde vitae!
          </p>
        </div>
      </a>
    </div>

    <p id="messageInfo">{{ infoMessage }}</p>

    <form @submit.prevent="searchMovie()" class="search-box">
      <input type="text" placeholder="O que procura?" v-model="search" />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="trending in news" :key="trending.url">
        <router-link :to="`/${trending.title}`" class="movie-link">
          <div class="product-image">
            <img :src="trending.image" />
            <div class="type">{{ trending.source.name }}</div>
          </div>
          <div class="detail">
            <p class="date">{{ trending.publishedAt }}</p>
            <h3>{{ trending.title }}</h3>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "../../env";

export default {
  setup() {
    const search = ref("");
    const news = ref([]);
    const infoMessage = ref("");

    const searchMovie = async () => {
      if (search.value != "") {
        fetch(`${env.apiUrl}${search.value}&lang=pt&max=10&token=${env.apiKey}`)
          .then(response => response.json())
          .then(data => {
            news.value = data.articles;
            localStorage.setItem("news:vue", JSON.stringify(data.articles));
          });
      } else {
        infoMessage.value = "Preencha o campo, por favor!";
      }
    };

    return {
      search,
      news,
      infoMessage,
      searchMovie
    };
  }
};
</script>

<style lang="scss">
.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0, 0, 0, 0.4);
      padding: 16px;
      z-index: 1;

      h3 {
        color: white;
        margin-bottom: 16px;
      }

      p {
        color: #ffffff;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: white;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: white;
        }

        &:focus {
          box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        color: white;
        font-size: 20px;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
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
    padding: 16px 8px;
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
          object-fit: cover;
        }
        .type {
          position: absolute;
          padding: 8px 16px;
          background-color: #42b883;
          color: #fff;
          bottom: 16px;
          left: 0px;
          text-transform: capitalize;
        }
      }
    }
  }
}

.detail {
  background-color: #496583;
  padding: 16px 8px;
  flex: 1 1 100%;
  border-radius: 0px 0px 8px 8px;
  .date {
    color: #aaa;
    font-size: 14px;
  }
  h3 {
    color: #fff;
    font-weight: 600;
    font-size: 18px;
  }
}

#messageInfo {
  color: rgb(172, 12, 12);
  text-align: center;
  margin-top: 10px;
}
</style>
