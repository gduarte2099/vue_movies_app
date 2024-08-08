<template>
  <div class="movie-detail">
    <!-- Mostrar mensaje de carga mientras loading es true -->
    <p v-if="loading">Loading...</p>

    <!-- Mostrar contenido cuando loading es false -->
    <div v-else>
      <h2>{{ movie.Title }}</h2>
      <p>{{ movie.Year }}</p>
      <p>{{ movie.Rated }}</p>
      <img :src="movie.Poster" alt="movie poster" class="featured-img" />
      <p>{{ movie.Plot }}</p>
    </div>
  </div>
</template>

<script >
/**Antes de que el elemento se monte en el DOM tenemos que hacer la peticion da la API */
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";
import env from "@/env.js";

export default {
  setup() {
    const movie = ref({}); // Referencia de la película que recibimos
    const loading = ref(true); // Estado de carga
    const route = useRoute();

    // Función para cargar datos
    async function fetchMovie() {
      try {
        const response = await fetch(
          `https://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`
        );
        const data = await response.json();
        movie.value = data;
      } catch (error) {
        console.error("Error fetching movie data:", error);
      } finally {
        loading.value = false; // Cambia el estado de carga a false al final
      }
    }

    onBeforeMount(() => {
      fetchMovie();
    });

    return {
      movie,
      loading,
    };
  },
};
</script>

<style lang="scss">
.movie-detail {
  padding: 16px;

  h2 {
    color: #fff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .featured-img {
    display: block;
    width: 100%;
    object-fit: contain;
    max-width: 200px;
    margin-bottom: 16px;
  }

  p {
    color: #fff;
    font-size: 18px;
    line-height: 1.4;
  }
}
</style>