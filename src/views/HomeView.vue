<template>
  <div>
    <div class="inputPokemon">
      <input type="text" placeholder="Enter Pokemon" v-model="text" />
    </div>
    <div
      class="pokemonResults"
      v-for="(pokemon, index) in filteredPokemonList"
      :key="index"
    >
      <router-link :to="`/about/${lookUpTable[pokemon.name]}`">
        {{ pokemon.name }}</router-link
      >
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import { reactive, toRefs, computed } from "vue";

export default {
  name: "HomeView",
  setup() {
    const state = reactive({
      pokemons: [],
      lookUpTable: {},
      text: "",
      filteredPokemonList: computed(() => updatePokemon()),
    });
    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }
    fetch("https://pokeapi.co/api/v2/pokemon?offset=0&limit=20")
      .then((res) => res.json())
      .then((data) => {
        state.pokemons = data.results;
        state.lookUpTable = data.results.reduce(
          (acc, current, index) =>
            (acc = { ...acc, [current.name]: index + 1 }),
          {}
        );
      });
    return { ...toRefs(state) };
  },
  components: {},
};
</script>
