<template>
  <div class="title">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand class="ml-2">
        <img src>
        <b style='color:black'>{{ appName}}</b>
      </b-navbar-brand>
    </b-navbar>

  <main>
    <b >{{"Como é o frete que você precisa ?"}}</b>
    <hr style="width: 350px" size="4px"> 

    <!--drop down-->
    <div class="Destino">
      
      <label for="cars">Destino:</label>

      <select name="cars" id="cars">
        <option value="volvo">Volvo</option>
        <option value="saab">Saab</option>
        <option value="mercedes">Mercedes</option>
        <option value="audi">Audi</option>
      </select>
    </div>

    <div class="Peso">
      <big>Peso</big>
      <div>
        <input type="number" placeholder="Insira aqui o peso da carga em Kg" style="width: 300px; font-size:large" >
      </div>
    </div>

    <div class="Botao">
      <button v-on:click="analise" style=" background-color: rgb(147, 196, 125) !important;"> Analisar </button>
    </div>

    <b >{{"Estas são as melhores alternativas de frete que encontramos pra você."}}</b>
    <hr style="width: 350px" size="4px"> 
    

    <div class="result">
      <p>Frete mais barato:{{cheaper}}</p>
      <p>Frete mais rapido:{{fastest}}</p>

    </div>
    
  </main>

 
  </div>


</template>

<script>


import {
  BNavbar,
  BNavbarBrand, 
} from 'bootstrap-vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
  },
  data() {
    const appName = ''
    const cheaper = ''
    const fastest = ''

    return {
      appName,
      cheaper,
      fastest,
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina
    this.appName = 'MELHOR FRETE'
    this.cheaper = "ain"
    this.fastest = "euin"
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    methodFoo() {
      console.log(this.appName)
    },

    analise(){
      this.cheaper = "pspspspspp"
      this.fastest = "dsidujishdji"
    },


  },
}
</script>

<style scoped>
.title .navbar {
  background-color: rgb(147, 196, 125) !important;
}
.title.hr{
  size: 50px
}
main{
  padding: 50px;
}
.title .navbar-brand {
  margin-left: 20px;
}
.Destino{
  margin-left: 5px;
  margin-top: 10px;
}

.Destino.label{
  size: 100px;
  font-size: larger;
}

.Peso{
  margin-left: 5px;
  margin-top: 50px;
  font-size: larger;
}

.Botao{
  padding: 40px;
  font-size: larger;
}
</style>
