<template>
  <div class="home">
    <div class="feature-card">
      <!--El link del router aqui va a un id dentro de la api, es decir, al id de una peli-->
      <!--cada ruta debe tener su camino, su path en el index-->
      <router-link :to="'/movie/' + link">
        <img
          :src="'./wall' + randomIndex + '.jpg'"
          alt="poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>{{ arrayWall[randomIndex].title }}</h3>
          <p>{{ arrayWall[randomIndex].sinopsis }}</p>
        </div>
      </router-link>
    </div>

    <!--submit.prevent previene el comportamiento predeterminado que es enviar el formulario.-->
    <form @submit.prevent="searchMovies()" class="search-box">
      <input type="text" placeholder="Type your movie..." v-model="search" />
      <input type="submit" value="Search" />
    </form>

    <!--MOVIES SEARCHED-->
    <div class="movie-list">
      <div v-if="noResults"><h3 class="c-white ml-10">Not movies found</h3></div>
      <div v-else class="movie" v-for="movie in movies" :key="movie.imdbID">
        <!--creamos un enlace a movies con el id de cada peli, para optener todos los datos del array-->
        <!--aqui to lo usamos con binding para que haga referencia a nuestro array de referencia-->
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <!--de manera simiar a router, aqui bindeamos el src para usar el array-->
            <img :src="movie.Poster" alt="movie-poster" />
            <div class="movie-type">{{ movie.Type }}</div>
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
import { onMounted } from "vue";
import env from "@/env.js";

//import { onBeforeMount } from 'vue'

export default {
  setup() {
    const search = ref(""); //string de referencia para buscar la peli, usamos v-model en el input
    const movies = ref([]); //array de las pelis optenidas con la referecia buscada
    let noResults = ref(false);

    let link = ref("");
    let arrayID = ["tt4154756", "tt6263850", "tt22022452"];
    let arrayWall = [
      {
        title: "Avengers: Infinity War",
        sinopsis:
          "The powerful Thanos, determined to obtain the Infinity Stones to control the universe, attacks the ship in which the survivors of Asgard are fleeing. His intention is to get hold of the second of the gems, now in the hands of Loki.",
      },
      {
        title: "Deadpool & Wolverine",
        sinopsis:
          "Wolverine is recovering from his injuries when he crosses paths with the loudmouth, Deadpool, who has traveled back in time to heal him in the hopes of becoming friends and forming a team to take down a common enemy.",
      },
      {
        title: "Inside-Out 2",
        sinopsis:
          "Now a teenager, Riley experiences new feelings like Anxiety and Envy, which take over the old ones as she hesitates about abandoning her old friends for others from high school.",
      },
    ];
    let randomIndex = ref(0);

    const searchMovies = () => {
      noResults.value = false;
      if (search.value != "") {
        //el usuario tipeo una peli, aqui usamos la API y hacemos el request con fetch
        //usamos el valor de la apikey y el valor de search para generar promesas
        //con data optenemos el json de la api
        try {
          fetch(
            `https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`
          )
            .then((response) => response.json())
            .then((data) => {
              movies.value = data.Search; //Search es el array de las pelis almacenadas en la api, se asiga a nuestro array de referencia
              search.value = "";

              if (!data.Search) {
                // Si la respuesta es False, no se encontraron resultados
                movies.value = [];
                noResults.value = true;
              }
            });
        } catch (error) {
          console.error("Error fetching movies:", error);
        }
      }
    };

    const getRandomID = () => {
      randomIndex.value = Math.floor(Math.random() * arrayID.length);
      link.value = arrayID[randomIndex.value];
    };

    onMounted(() => {
      getRandomID();
    });

    console.log("Desarrollado por gduarte2999@gmail.com");

    return {
      search,
      movies,
      searchMovies,
      getRandomID,
      arrayWall,
      link,
      randomIndex,
      noResults,
    };
  },
};
</script>

<style lang="scss">
.detail {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.6);
  padding: 16px;
  z-index: 1;
}

.featured-img {
  display: block;
  width: 100%;
  height: 300px;
  object-fit: cover;

  position: relative;
  z-index: 0;
}

h3 {
  color: #fff;
  margin-bottom: 16px;
}

p {
  color: #fff;
}

.c-white {
  color: #fff;
}

.ml-10{
  margin-left: 10px;
}
</style>

<style lang="scss">
.home {
  /*BANNER PRINCIPAL*/
  .feature-card {
    /*En position: relative , 
    el elemento está posicionado Relativo a sí mismo. Sin embargo, un elemento absolutamente posicionado es relativo a su padre */
    /*El posicionamiento absolute hace que un elemento se coloque respecto a su contenedor posicionado mas cercano, 
    si no encuentra ningún contenedor cercano, el elemento se colocara respecto al viewport. */
    position: relative;
  }

  /**SECCION DE BUSQUEDA */
  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      //estilos para los inputs en general
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        //este es el input de tipo texto, es decir el placeholder
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &::focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  /**SECCCION LISTADO DE PELICULAS */
  .movie-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin: 0px 8px;

    .movie {
      width: 30%; /* Ajusta el ancho al 30% para mostrar 3 películas por fila */
      margin-bottom: 20px; /* Añade espacio entre las filas */
      padding: 16px 8px;
    }

    /* Media Query para pantallas pequeñas */
    @media (max-width: 768px) {
      .movie {
        width: 100%; /* Ocupa el 100% del ancho en pantallas pequeñas */
      }
    }

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
          // height: 275px;
          object-fit: fill;
        }

        .movie-type {
          position: absolute;
          padding: 8px 16px;
          background-color: #42b883;
          color: #fff;
          bottom: 16px;
          left: 0px;
          text-transform: capitalize;
        }
      }

      .movie-detail {
        background-color: #496583;
        padding: 16px 8px;
        flex: 1 1 100%;
        border-radius: 0px 0px 8px 8px; //cero arriba, 0 der, 8 abajo, 8 izq
      }

      .movie-year {
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
</style>
