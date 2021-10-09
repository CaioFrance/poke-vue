<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <h3 class="is-size-3">Pokedex</h3>
      <input
        @keyup.enter="buscar"
        class="input is-rounded"
        type="text"
        placeholder="buscar pokemon"
        v-model="busca"
      />
      <button class="button is-success is-rounded mt-2" @click="buscar">
        Buscar
      </button>
      <div v-for="(pokemon, index) in filteredPokemons" :key="pokemon.url">
        <Pokemon :name="pokemon.name" :id="index + 1" :url="pokemon.url" />
      </div>
    </div>
  </div>
</template>

<script>
import api from "./services/api";

import Pokemon from "./components/Pokemon";

export default {
  name: "App",
  components: {
    Pokemon,
  },
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: "",
      buscaNull: false,
    };
  },
  methods: {
    buscar() {
      if (this.busca.trim() === "") {
        this.filteredPokemons = this.pokemons;
        this.busca = "";
        return this.filteredPokemons;
      }
      this.filteredPokemons = this.pokemons.filter(
        (pokemon) => pokemon.name === this.busca
      );
      this.busca = "";
      if (this.filteredPokemons === []) {
        this.buscaNull = true;
      }
      return this.filteredPokemons;
    },
  },
  computed: {
    // resultBusca() {
    //   if (this.busca.trim() === "") {
    //     return this.pokemons;
    //   }
    //   return this.pokemons.filter((pokemon) => pokemon.name === this.busca);
    // },
  },
  created() {
    api.get(`pokemon?limit=151&offset=0`).then((res) => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
