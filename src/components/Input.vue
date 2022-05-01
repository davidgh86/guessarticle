<template>
  <div>
    <label>
      Palabra 
    </label>
    <input
      type="text"
      v-model="modelValue"
      @keyup.enter="handleSubmit"
    />
    <button type="button" @click="handleSubmit">OK</button>
  </div>
</template>

<script>
import {ref} from "vue"

export default {
  name: "input-word",
  setup(props, {emit}) {
    const modelValue = ref("");
    function handleSubmit(e) {
      e.preventDefault();
      emit("submitedWord", this.modelValue)
      this.modelValue = ""
    }
    return {
      modelValue,
      handleSubmit
    }
  }
};
</script>