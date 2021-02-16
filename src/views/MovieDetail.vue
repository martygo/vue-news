<template>
  <div class="news-detail">
    <h2>{{ article.title }}</h2>
    <p>{{ article.description }}</p>
    <img :src="article.image" class="featured-img" />
    <p>{{ article.content }}</p>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import env from "../../env";

export default {
  setup() {
    const article = ref({});
    const route = useRoute();

    const urlRoute = route.params.id;

    onBeforeMount(() => {
      fetch(`${env.apiUrl}${urlRoute}&lang=pt&max=10&token=${env.apiKey}`)
        .then(response => response.json())
        .then(data => {
          article.value = data.articles[0];
        });
    });

    return {
      article,
      route
    };
  }
};
</script>

<style lang="scss">
.news-detail {
  padding: 16px;
  h2 {
    color: #fff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }
  .featured-img {
    display: block;
    max-width: 400px;
    margin-top: 30px;
    margin-bottom: 30px;
  }
  p {
    color: #fff;
    font-size: 18px;
    line-height: 1.4;
  }
}
</style>
