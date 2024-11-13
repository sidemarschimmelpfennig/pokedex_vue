<template>
  <div class="pokemon" style="width: 400px">
    <div
      class="max-w-sm rounded overflow-hidden shadow-lg bg-white items-center justify-center justify-items-center"
    >
      <div class="flex justify-items-end items-center space-x-4">
        <!-- Imagem -->
        <img
          :src="currentImg"
          @click="changeSprite"
          alt="Descrição da imagem"
          class="rounded-full w-full hover:cursor-pointer"
          style="height: 150px; width: 150px"
        />
      </div>

      <div class="px-6 py-4">
        <div class="font-bold text-xl mb-2">{{ upperCase(name) }}</div>
      </div>
      <div class="px-6 pt-4 pb-2">
        <span
          class="inline-block rounded-full px-3 py-1 text-sm font-semibold text-white mr-2 mb-2"
          :class="isColor"
          >Tipo: {{ pokemon.type }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
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
      isColor: "",
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
        return "planta";
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
        return "venenoso";
      } else if (type.toLowerCase() === "fairy") {
        return "fada";
      } else if (type.toLowerCase() === "fighting") {
        return "lutador";
      } else if (type.toLowerCase() === "rock") {
        return "rocha";
      } else if (type.toLowerCase() === "ghost") {
        return "fantasma";
      } else if (type.toLowerCase() === "steel") {
        return "metal";
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
        return (this.currentImg = this.pokemon.back);
      } else {
        this.isFront = true;
        return (this.currentImg = this.pokemon.front);
      }
    },
    colorClass(value) {
      switch (value) {
        case "planta":
          return (this.isColor = "bg-green-800");
        case "inseto":
          return (this.isColor = "bg-lime-500");
        case "água":
          return (this.isColor = "bg-blue-500");
        case "fogo":
          return (this.isColor = "bg-orange-600");
        case "elétrico":
          return (this.isColor = "bg-yellow-500");
        case "rocha":
          return (this.isColor = "bg-yellow-950");
        case "venenoso":
          return (this.isColor = "bg-violet-600");
        case "fada":
          return (this.isColor = "bg-red-300");
        case "sombrio":
          return (this.isColor = "bg-black");
        case "psíquico":
          return (this.isColor = "bg-red-500");
        case "lutador":
          return (this.isColor = "bg-red-800");
        case "metal":
          return (this.isColor = "bg-stone-500");
        case "terra":
          return (this.isColor = "bg-amber-700");
        case "fantasma":
          return (this.isColor = "bg-violet-900");
        default:
          return (this.isColor = "bg-neutral-400");
      }
    },
  },
  created() {
    axios.get(this.url).then((res) => {
      this.pokemon.type = this.translateType(res.data.types[0].type.name);
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
      this.colorClass(this.pokemon.type);
    });
  },
};
</script>

<style scoped>
.pokemon {
  margin-top: 2%;
}
</style>
