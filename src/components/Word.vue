<template>
  <div class="hello">
    {{showValue}}
  </div>
</template>

<script>
import {computed} from "vue"

function a(val) {
  let result = "";
  for (let i=0; i<val.length; i++){
    result += "_" //"█"
  }
  return result;
}

export default {
  name: 'WordElement',
  props: {
    value: String,
    current: String,
  },
  setup(props) {
      let revealed = false;
      const mask = a(props.value);
      const matchWords = (word1, word2) => {
        return word1.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "") === word2.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "");
      }
      const showValue = computed(()=>{
      if (revealed || matchWords(props.value, props.current)){
        if (!revealed) {
          revealed = true;
          //emit("guessed", )
        }
        return props.value, props.value;
      } else{
        return mask;
      }
    });
    return {
      showValue
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
