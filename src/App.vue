<template>
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
  <div class="footer">
  <p><Input @submitedWord="submitWord" class="footer"/></p>
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
      guessedWords: !localStorage.getItem("guessedWords")?new Set([]):new Set(JSON.parse(localStorage.getItem("guessedWords"))),
      title: ["Hospital"," ","de"," ","Santa"," ","María"," ","Magdalena"," ","(","Fuentidueña",")"],
      propositions: new Set(["a", "ante", "bajo", "cabe", "con", "contra", "de", "desde", "durante", "en ", "entre", "hacia", "hasta", "mediante", "para", "por", "según", "sin", "so", "sobre", "tras", "versus", "vía"]),
      separators: new Set(["\n", "(", ")", " ", ",", ".", ";", ":", "_", "“", "”"]),
      words : ["El"," ","Hospital"," ","de"," ","Santa"," ","María"," ","Magdalena"," ","fue"," ","un"," ","recinto"," ","hospitalario"," ","fundado"," ","en"," ","el"," ","año"," ","1540"," ","por"," ","doña"," ","Mencía"," ","de"," ","Mendoza","."," ","Se"," ","encuentran"," ","a"," ","las"," ","afueras"," ","de"," ","la"," ","localidad"," ","de"," ","Fuentidueña",","," ","provincia"," ","de"," ","Segovia",","," ","(","España",")","."," ","Es"," ","un"," ","Bien"," ","de"," ","Interés"," ","Cultural"," ","del"," ","patrimonio"," ","histórico"," ","de"," ","España",","," ","su"," ","estado"," ","es"," ","de"," ","ruina",".","\n","Historia","\n","","\n","El"," ","Hospital"," ","de"," ","Santa"," ","María"," ","Magdalena"," ","fue"," ","fundado"," ","en"," ","el"," ","año"," ","de"," ","1540",","," ","por"," ","doña"," ","Mencía"," ","de"," ","Mendoza",","," ","que"," ","dejó"," ","en"," ","su"," ","testamento"," ","los"," ","medios"," ","necesarios","."," ","Estuvo"," ","en"," ","funcionamiento"," ","hasta"," ","mediados"," ","del"," ","siglo"," ","XIX"," ","(","año"," ","de"," ","1853",")"," "," ","en"," ","que"," ","fue"," ","incautado"," ","por"," ","el"," ","Estado"," ","y"," ","anulado"," ","su"," ","uso","."," ","Fue"," ","un"," ","centro"," ","asistencial"," ","de"," ","gran"," ","importancia",","," ","ejemplo"," ","del"," ","método"," ","sanitario"," ","aplicado"," ","durante"," ","la"," ","Edad"," ","Moderna"," ","y"," ","que"," ","tan"," ","notables"," ","servicios"," ","otorgó"," ","a"," ","la"," ","sociedad"," ","civil"," ","de"," ","la"," ","época",".","\n","Dentro"," ","de"," ","la"," ","tipología"," ","de"," ","hospitales"," ","renacentistas"," ","constituye"," ","igualmente"," ","un"," ","caso"," ","ejemplar",","," ","por"," ","la"," ","capacidad"," ","y"," ","calidad"," ","de"," ","sus"," ","servicios"," ","y"," ","por"," ","la"," ","conformación"," ","de"," ","sus"," ","distintas"," ","dependencias","."," ","Los"," ","nuevos"," ","métodos"," ","sanitarios"," ","del"," ","siglo"," ","XIX"," ","provocaron"," ","su"," ","abandono"," ","y"," ","posterior"," ","ruina",","," ","aunque"," ","esta"," ","no"," ","se"," ","produjo"," ","masivamente"," ","hasta"," ","muy"," ","avanzado"," ","el"," ","siglo"," ","XX",".","\n","","\n","Descripción","\n","Se"," ","levantó"," ","adherido"," ","a"," ","la"," ","muralla",","," ","junto"," ","a"," ","la"," ","iglesia"," ","de"," ","San"," ","Miguel"," ","y"," ","en"," ","las"," ","proximidades"," ","del"," ","castillo",","," ","en"," ","el"," ","lado"," ","noroeste"," ","de"," ","la"," ","localidad","."," ","Y"," ","su"," ","advocación"," ","a"," ","María"," ","Magdalena"," ","remite"," ","a"," ","su"," ","labor"," ","y"," ","generosidad",".","\n","","\n","En"," ","planta"," ","se"," ","componía"," ","de"," ","un"," ","gran"," ","cuadrángulo"," ","con"," ","patio"," ","cerrado"," ","con"," ","columnas"," ","y"," ","con"," ","acceso"," ","por"," ","puertas"," ","a"," ","dos"," ","lados"," ","y"," ","rodeado"," ","de"," ","dos"," ","galerías",","," ","que"," ","estaba"," ","en"," ","pie"," ","todavía"," ","en"," ","1932","."," ","Alrededor"," ","de"," ","estas"," ","se"," ","disponían"," ","las"," ","sucesivas"," ","dependencias","."," ","En"," ","la"," ","esquina"," ","habitada"," ","se"," ","ubicaba"," ","la"," ","capilla"," ","con"," ","arcos"," ","rebajados"," ","abiertos"," ","a"," ","las"," ","naves"," ","transversales",","," ","a"," ","la"," ","altura"," ","de"," ","las"," ","tres"," ","plantas"," ","para"," ","su"," ","visión"," ","directa",","," ","siendo"," ","así"," ","un"," ","sistema"," ","simplificado"," ","a"," ","la"," ","mitad"," ","de"," ","los"," ","hospitales"," ","en"," ","cruz"," ","de"," ","los"," ","Reyes"," ","Católicos"," ","y"," ","el"," ","gran"," ","cardenal"," ","don"," ","Pedro"," ","González"," ","de"," ","Mendoza",","," ","de"," ","finales"," ","del"," ","siglo"," ","XV"," ","y"," ","comienzos"," ","del"," ","siglo"," ","XVI","."," ","Así"," ","pues",","," ","junto"," ","a"," ","la"," ","capilla"," ","–de"," ","planta"," ","cuadrada-"," ","se"," ","abrían"," ","dos"," ","crujías"," ","en"," ","ángulo"," ","recto"," ","o"," ","forma"," ","de"," ","“","“","L","”"," ","con"," ","dependencias"," ","para"," ","las"," ","distintas"," ","clases"," ","de"," ","enfermos","."," ","Hoy",","," ","apenas"," ","se"," ","conservan"," ","restos"," ","de"," ","aquellas",","," ","como"," ","sus"," ","dos"," ","grandes"," ","arcos"," ","y"," ","algunos"," ","muros"," ","en"," ","el"," ","costado"," ","norte",".","\n","","\n","Estaba"," ","construido"," ","en"," ","buena"," ","piedra"," ","caliza",","," ","en"," ","sillares"," ","y"," ","en"," ","mampostería",".","\n","","\n","Tenía"," ","tres"," ","plantas","."," ","En"," ","el"," ","muro"," ","norte"," ","se"," ","advierte"," ","el"," ","sistema"," ","constructivo"," ","empleado"," ","con"," ","sillería"," ","de"," ","piedra"," ","y"," ","mampostería"," ","al"," ","interior","."," ","La"," ","planta"," ","baja"," ","tenía"," ","menos"," ","aberturas",","," ","dos"," ","grandes"," ","vanos"," ","de"," ","arco"," ","de"," ","medio"," ","punto",","," ","en"," ","la"," ","planta"," ","siguiente"," ","ventanas"," ","de"," ","arco"," ","rebajado"," ","y"," ","en"," ","la"," ","superior"," ","ocho"," ","ventanas"," ","rectangulares"," ","que"," ","comunicaban"," ","con"," ","las"," ","celdas"," ","corridas"," ","y"," ","abiertas",".","\n","","\n","En"," ","la"," ","fachada"," ","norte"," ","se"," ","observa"," ","hasta"," ","cuatro"," ","alturas",","," ","tres"," ","de"," ","funcionalidad"," ","hospitalaria"," ","y"," ","un"," ","sótano"," ","de"," ","almacén",".","\n","","\n","Se"," ","conserva"," ","una"," ","puerta"," ","de"," ","acceso"," ","en"," ","el"," ","muro"," ","oriental"," ","con"," ","arco"," ","de"," ","medio"," ","punto"," ","y"," ","dovelas"," ","de"," ","buen"," ","tamaño",".","\n","","\n","En"," ","la"," ","capilla",","," ","los"," ","arcos"," ","arruinados"," ","se"," ","levantaban"," ","sobre"," ","pilares"," ","de"," ","columnas"," ","dobles"," ","adosadas"," ","con"," ","capitel"," ","sencillo"," ","de"," ","referencia"," ","al"," ","estilo"," ","toscano"," ","de"," ","la"," ","primera"," ","mitad"," ","del"," ","siglo"," ","XVI","."],
      submittedInput: ""
    }
  },
  mounted(){
    //console.log(JSON.stringify(localStorage.getItem("guessedWords")))
    //console.log("sfdfas " + [...this.guessedWords])
    // if(localStorage.getItem("guessedWords")){

    //   this.guessedWords=new Set(JSON.parse(localStorage.getItem("guessedWords")));
    // } else {
    //   this.guessedWords=new Set([]);
    // }
  },
  methods: {
    submitWord(e){
      let standarized = this.getStandarizedWord(e)
      this.guessedWords.add(standarized);
      //console.log("------>" + [...this.guessedWords])
      localStorage.setItem("guessedWords", JSON.stringify([...this.guessedWords]));
      this.submittedInput = e
    },
    getStandarizedWord(word){
      return word.trim().toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "")
    },
    isGuessable(word){
      return !this.propositions.has(word.toLowerCase()) && !this.separators.has(word) && !this.guessedWords.has(this.getStandarizedWord(word))
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
