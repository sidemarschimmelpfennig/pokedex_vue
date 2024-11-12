<template>
  <div class="flex flex-col items-center min-h-screen bg-slate-400" id="app">
    <NavBar @update:search="searchPokemon" />
    <div class="mt-28"></div>
    <div
      v-for="(pokemon, id) in filteredPokemons"
      :key="pokemon.url"
      class="mb-4"
      id="content"
    >
      <PokemonComponent :name="pokemon.name" :id="id" :url="pokemon.url" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import NavBar from "./components/NavBar.vue";
import PokemonComponent from "./components/PokemonComponent.vue";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      searchQuery: "", // Armazena o valor de busca
    };
  },
  computed: {
    filteredPokemons() {
      if (!this.searchQuery) {
        return this.pokemons;
      }
      return this.pokemons.filter((pokemon) => {
        return pokemon.name
          .toLowerCase()
          .includes(this.searchQuery.toLowerCase());
      });
    },
  },
  created() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
      });
  },
  methods: {
    searchPokemon(query) {
      this.searchQuery = query;
    },
  },
  components: {
    PokemonComponent,
    NavBar,
  },
};
</script>
