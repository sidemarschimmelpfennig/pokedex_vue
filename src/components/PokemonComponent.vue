<template>
  <div class="pokemon" style="width: 200px">
    <div class="max-w-sm rounded overflow-hidden shadow-lg bg-white">
      <div class="flex justify-items-end items-center space-x-4">
        <!-- Imagem -->
        <ArrowLeftIcon
          class="px-2 py-4 bg-white text-black-200 rounded hover:bg-gray-100 hover:cursor-pointer w-10 h-full"
          :class="{ hidden: isClassVisible }"
          @click="changeSprite"
        />
        <img
          :src="currentImg"
          alt="Descrição da imagem"
          class="rounded-full w-full"
        />

        <ArrowRightIcon
          class="px-2 py-4 bg-white text-black-200 rounded hover:bg-gray-100 hover:cursor-pointer w-10 h-11"
          @click="changeSprite"
          :class="{ hidden: !isClassVisible }"
        />
      </div>

      <div class="px-6 py-4">
        <div class="font-bold text-xl mb-2">{{ upperCase(name) }}</div>
      </div>
      <div class="px-6 pt-4 pb-2">
        <span
          class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
          >Tipo: {{ pokemon.type }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import { ArrowLeftIcon, ArrowRightIcon } from "@heroicons/vue/24/solid";
import axios from "axios";
export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    url: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      isFront: true,
      isClassVisible: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  methods: {
    upperCase(value) {
      if (!value) return "";
      return value[0].toUpperCase() + value.slice(1);
    },
    translateType(type) {
      if (type.toLowerCase() === "grass") {
        return "grama";
      } else if (type.toLowerCase() === "fire") {
        return "fogo";
      } else if (type.toLowerCase() === "water") {
        return "água";
      } else if (type.toLowerCase() === "electric") {
        return "elétrico";
      } else if (type.toLowerCase() === "normal") {
        return "normal";
      } else if (type.toLowerCase() === "psychic") {
        return "psíquico";
      } else if (type.toLowerCase() === "bug") {
        return "inseto";
      } else if (type.toLowerCase() === "flying") {
        return "voador";
      } else if (type.toLowerCase() === "poison") {
        return "veneno";
      } else if (type.toLowerCase() === "fairy") {
        return "fada";
      } else if (type.toLowerCase() === "fighting") {
        return "luta";
      } else if (type.toLowerCase() === "rock") {
        return "rocha";
      } else if (type.toLowerCase() === "ghost") {
        return "fantasma";
      } else if (type.toLowerCase() === "steel") {
        return "aço";
      } else if (type.toLowerCase() === "ice") {
        return "gelo";
      } else if (type.toLowerCase() === "dragon") {
        return "dragão";
      } else if (type.toLowerCase() === "dark") {
        return "sombrio";
      } else if (type.toLowerCase() === "ground") {
        return "terra";
      } else {
        return type; // Retorna o tipo original caso não haja tradução
      }
    },
    changeSprite() {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
        this.isClassVisible = false;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
        this.isClassVisible = true;
      }
    },
  },
  created: function () {
    axios.get(this.url).then((res) => {
      this.pokemon.type = this.translateType(res.data.types[0].type.name);
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    });
  },
  components: {
    ArrowRightIcon,
    ArrowLeftIcon,
  },
};
</script>

<style scoped>
.pokemon {
  margin-top: 2%;
}
</style>
