<template>
  <div>
    {{ wordsRegistry }}
    <div  v-if="!finished">
      <div v-for="w in title" :key="w" style="display:inline-flex;">
        <Word v-if="isGuessable(w)" :value="w" :current="submittedInput" style="display:inline-flex;" />
        <br v-else-if="w === '\n'">
        <div v-else-if="w === ' '">&nbsp;</div>
        <div v-else style="display:inline-flex;">{{ w }}</div>
      </div>
      <br/>
      <br/>
      <br/>
      <div v-for="w in words" :key="w" style="display:inline-flex;">
        <Word v-if="isGuessable(w)" :value="w" :current="submittedInput" style="display:inline-flex;" />
        <br v-else-if="w === '\n'">
        <div v-else-if="w === ' '">&nbsp;</div>
        <div v-else style="display:inline-flex;">{{ w }}</div>
      </div>
      <br/>
      <br/>
      <br/>
      <div class="footer" v-if="!finished">
      <p><Input @submitedWord="submitWord" class="footer"/></p>
      </div>
    </div>
    <div  v-if="finished">
      <div v-for="w in title" :key="w" style="display:inline-flex;">
        <br v-if="w === '\n'">
        <div v-else-if="w === ' '">&nbsp;</div>
        <div v-else style="display:inline-flex;">{{ w }}</div>
      </div>
      <br/>
      <br/>
      <br/>
      <div v-for="w in words" :key="w" style="display:inline-flex;">
        <br v-if="w === '\n'">
        <div v-else-if="w === ' '">&nbsp;</div>
        <div v-else style="display:inline-flex;">{{ w }}</div>
      </div>
    </div>
  </div>
  
</template>

<script>
import Word from './components/Word.vue'
import Input from './components/Input.vue'
//import {ref} from "vue"

export default {
  name: 'App',
  components: {
    Word,
    Input
  },
  data() {
    return {
      finished: false,
      wordsRegistry: {},
      guessedWords: new Set([]),
      title: "a, ,b, ,c, ,d".split(","),
      propositions: new Set(["a", "ante", "bajo", "cabe", "con", "contra", "de", "desde", "durante", "en ", "entre", "hacia", "hasta", "mediante", "para", "por", "según", "sin", "so", "sobre", "tras", "versus", "vía"]),
      separators: new Set(["\n", "(", ")", " ", ",", ".", ";", ":", "_", "-","“", "”"]),
      words : "e, ,f, ,g, ,h".split(","),
      submittedInput: ""
    }
  },
  mounted(){
    let lastFinishedArticle = !localStorage.getItem("lastArticle")?[]:JSON.parse(localStorage.getItem("lastArticle"));
    if (this.isSameArray(lastFinishedArticle, this.title)){
      this.finished = true
    } else {
      localStorage.removeItem("wordsRegistry")
      localStorage.removeItem("guessedWords")
    }
    this.wordsRegistry = !localStorage.getItem("wordsRegistry")?{}:JSON.parse(localStorage.getItem("wordsRegistry"));
    this.guessedWords = !localStorage.getItem("guessedWords")?new Set([]):new Set(JSON.parse(localStorage.getItem("guessedWords")));
  },
  methods: {
    submitWord(e){
      let standarized = this.getStandarizedWord(e)
      this.guessedWords.add(standarized);
      localStorage.setItem("guessedWords", JSON.stringify([...this.guessedWords]));
      
      this.submittedInput = e
      this.appendNewWord(e)
      if (this.guessedTitleWords()){
        this.finished = true
        localStorage.setItem("lastArticle", JSON.stringify(this.title))
      }
      
    },
    appendNewWord(word) {
      let elementsCount = this.countWords(word)
      let firstApperared = this.fistWordAppered(word)
      let key = this.getStandarizedWord(firstApperared)

      let object = {
        word: firstApperared,
        frequency: elementsCount
      }
      if (!(key in this.wordsRegistry)) {
        localStorage.setItem("wordsRegistry", JSON.stringify(this.wordsRegistry))
        this.wordsRegistry[key] = object
      }
    },
    getStandarizedWord(word){
      return word.trim().toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "")
    },
    isGuessable(word){
      return !this.propositions.has(word.toLowerCase()) && !this.separators.has(word) && !this.guessedWords.has(this.getStandarizedWord(word))
    },
    isSameArray(s1, s2){
      if (s1.length !== s2.length) {
        return false;
      }
      for(let i=0; i<s1.length; i++){
        if(this.getStandarizedWord(s1[i])!==this.getStandarizedWord(s2[i])){
          return false;
        }
      }
      return true;
    },
    guessedTitleWords(){
      return this.title.filter(i=>(i.length!=0 && !this.separators.has(i))).every(i => this.guessedWords.has(this.getStandarizedWord(i)))
    },
    countWords(word){
      let wordsInTitle = this.title.filter(w => this.getStandarizedWord(w)===this.getStandarizedWord(word)).length;
      let wordsInWords = this.words.filter(w => this.getStandarizedWord(w)===this.getStandarizedWord(word)).length;
      return wordsInTitle + wordsInWords;
    },
    fistWordAppered(word){
      for (let w of this.words) {
        if (this.getStandarizedWord(w)===this.getStandarizedWord(word)){
          return w.toLowerCase()
        }
      }
      return word.toLowerCase()
    }

  }
}
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
.footer {
   position: fixed;
   left: 0;
   bottom: 0;
   padding: 2rem;
   width: 100%;
   height: 10%;
   background-color: rgb(20, 38, 176);
   color: white;
   text-align: center;
}
</style>
