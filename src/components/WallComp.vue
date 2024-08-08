<template>
  <!--El link del router aqui va a un id dentro de la api, es decir, al id de una peli-->
  <!--cada ruta debe tener su camino, su path en el index-->
  <router-link :to="'/movie/' + link">
    <img
      :src="'../assets/walls/wall' + randomIndex + '.jpg'"
      alt="avergers-poster"
      class="featured-img"
    />
    <div class="detail">
      <h3>Avengers</h3>
      <p>
        The powerful Thanos, determined to obtain the Infinity Stones to control
        the universe, attacks the ship in which the survivors of Asgard are
        fleeing. His intention is to get hold of the second of the gems, now in
        the hands of Loki.
      </p>
    </div>
  </router-link>
</template>

<script setup>
import { ref, onMounted } from "vue"

let link = ref("");
let arrayID = ["tt4154756", "tt6263850", "tt22022452"];

function getRandomID() {
  let randomIndex = Math.floor(Math.random() * arrayID.length);
  link.value = arrayID[randomIndex];

  console.log(link);
}

// Llama a la función para generar un ID aleatorio al montar el componente
onMounted(() => {
  getRandomID();
});
</script>

