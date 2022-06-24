<template>
  <button class="btn-voltar" @click="back">Voltar</button>
  <my-loader v-if="isLoading" />
  <div v-if="info">
    <div class="div-row">
      <img :src="image" />
      <h1>{{ info.name }}</h1>
    </div>

    <div class="div-row">
      <h4>Experiência: </h4>
      {{ info.base_experience }}
    </div>

    <div class="div-row">
      <h4>Peso: </h4>
      {{ info.weight }}
    </div>

    <div class="div-row">
      <h4>Altura: </h4>
      {{ info.height }}
    </div>

    <div class="div-images">
    <div v-for="(value, key, index) in info.sprites" :key="index">
        <img v-if="value && typeof value === 'string'" :src="value">
    </div>
    </div>
  </div>
</template>

<script>
import { useRoute, useRouter } from "vue-router";
import { onMounted, ref, computed } from "vue";
import { getPokemon, getPokemonImageUrl } from "../service/pokemon-service.js";
import MyLoader from "./MyLoader.vue";

export default {
  components: {
    MyLoader,
  },

  setup() {
    const router = useRouter();
    const route = useRoute();
    const info = ref(null);
    const isLoading = ref(false);

    onMounted(() => {
      isLoading.value = true;
      getPokemon(route.params.id)
        .then((resp) => {
          info.value = resp;
        })
        .finally(() => {
          isLoading.value = false;
        });
    });

    const back = () => router.replace("/");

    const image = computed(() => getPokemonImageUrl(info.value.id));

    return { isLoading, info, image, back };
  },
};
</script>

<style scoped>
.div-row {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.btn-voltar {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 11px 29px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 13px;
  border-radius: 0.3rem;
  cursor: pointer;
}
.div-images {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
}
</style>