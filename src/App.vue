<template>
  <div class="flex flex-col items-center min-h-screen bg-yellow-200" id="app">
    <NavBar @update:search="searchPokemon" />
    <div class="mt-28"></div>
    <NotFound v-if="filteredPokemons().length == 0" />
    <div
      v-for="(pokemon, id) in filteredPokemons()"
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
import NotFound from "./components/NotFound.vue";
import PokemonComponent from "./components/PokemonComponent.vue";
export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      pokemonsCopy: [],
      searchQuery: "", // Armazena o valor de busca
    };
  },
  created() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
        this.pokemonsCopy = this.pokemons;
      });
  },
  methods: {
    searchPokemon(query) {
      this.searchQuery = query;
    },
    filteredPokemons() {
      this.pokemonsCopy = this.pokemons;
      if (!this.searchQuery) {
        return this.pokemonsCopy;
      }
      return this.pokemonsCopy.filter((pokemon) => {
        return pokemon.name
          .toLowerCase()
          .includes(this.searchQuery.toLowerCase());
      });
    },
  },
  components: {
    PokemonComponent,
    NavBar,
    NotFound,
  },
};
</script>
