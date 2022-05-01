<template>
  <div>
    <label v-if="label" :for="id" class="font-bold block">
      Label
    </label>
    {{modelValue}}
    <input
      type="text"
      v-model="modelValue"
      @keyup.enter="handleSubmit"
    />
    <button type="button" @click="handleSubmit">OK</button>
  </div>
</template>

<script>

export default {
  name: "input-word",
  setup() {  
    return {
      modelValue: ""
    }
  },
  methods: {
     handleSubmit: function(e) {
      e.preventDefault();
      alert(this.modelValue);
    }
  }
};
</script>