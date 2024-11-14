<template>
  <div class="flex h-14 bg-red-400 w-full justify-start">
    <img src="@/assets/img/logo.png" alt="" class="h-14 pr-12 pl-2" />
    <div class="sm:col-span-4 pt-1">
      <div class="mt-2 relative">
        <span
          class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400"
          @click="triggerSearch"
        >
          <MagnifyingGlassIcon
            class="h-4 w-4 hover:text-black hover:cursor-pointer"
          />
        </span>
        <input
          id="search"
          name="search"
          type="text"
          placeholder="Busque seu Pokemon"
          class="block w-full pl-10 rounded-md border-none py-1.5 text-gray-900 ring-gray-300 placeholder:text-gray-400 sm:text-sm focus:outline-none"
          v-model="searchValue"
          @keyup.enter="triggerSearch"
          autocomplete="off"
        />
      </div>
    </div>
  </div>
</template>
  
  <script>
import { MagnifyingGlassIcon } from "@heroicons/vue/24/solid";
export default {
  components: {
    MagnifyingGlassIcon,
  },
  data() {
    return {
      searchValue: "",
    };
  },
  methods: {
    triggerSearch(event) {
      event.preventDefault();
      this.$emit("update:search", this.searchValue);
    },
  },
};
</script>
  