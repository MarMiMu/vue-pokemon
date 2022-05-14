<template>
  <div class="about">
    <div v-if="pokemon">
      <h3>{{ pokemon.name }}</h3>
      <img :src="pokemon.sprites.front_default" alt="" />
      <img :src="pokemon.sprites.back_default" alt="" />
      <p v-for="(type, index) in pokemon.types" :key="index">
        {{ type.type.name }}
      </p>
    </div>
  </div>
</template>

<script>
import { useRoute } from "vue-router";
import { reactive, toRefs } from "vue";
export default {
  setup() {
    const route = useRoute();
    const state = reactive({
      pokemon: null,
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemon = data;
      });
    return { ...toRefs(state) };
  },
};
</script>