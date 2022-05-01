<template>
  <div class="grid-container">
    <div v-for="item in wordRegistryList" class="grid-item" :key="item">
      {{ item }}
    </div>
  </div>
</template>

<script>
//import {ref} from "vue"
import { computed } from "vue";

export default {
  name: "word-registry",
  props: {
    registry: Map
  },
  setup(props) {
    const wordRegistryList = computed(() => {
      let result = []
      const reversedRegistry = Array.from(props.registry).reverse();
      // eslint-disable-next-line no-unused-vars
      reversedRegistry.forEach(([key, value]) => {
        result.push(value.numberOfGuessedWords)
        result.push(value.word)
        result.push(value.frequency)
      });
      return result;
    });
    return {
      wordRegistryList
    }
  }
};
</script>

<style>
.grid-container {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-gap: 0.1em;
}
.grid-item {
  /* grid-column: 1;
  grid-row: 1; */
}

</style>