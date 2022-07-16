<template>
  <div class="title">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand class="ml-2">
        <img class="logo" src="../assets/logo.png">
        <b style='color:black'>{{ appName}}</b>
      </b-navbar-brand>
    </b-navbar>

  <main>
    <b >{{"Como é o frete que você precisa ?"}}</b>
    <hr style="width: 350px" size="4px"> 

    <!--drop down-->
    <div class="Destino">
      
      <p> Destino</p>
      <div>
        <select v-model="dest_city" class="select-selected" id="cccity" name="cccity"></select><br>
      </div>
    </div>

    <div class="Peso">
      <p>Peso</p>
      <div>
        <input  v-model="weight" type="number" placeholder="Insira aqui o peso da carga em Kg" style="width: 300px; font-size:large" >

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
var select_pHolder = "Selecione aqui o destino do frete"
import {
  BNavbar,
  BNavbarBrand, 
} from 'bootstrap-vue'
import axios from 'axios'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
  },
  data() {
    const appName = ''
    const cheaper = ''
    const fastest = ''
    const weight = '';
    const dest_city = '';

    return {
      appName,
      cheaper,
      fastest,
      api_data:undefined,
      weight,
      dest_city
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina
    axios.get("http://localhost:3000/transport").then((resp) =>{
      this.api_data= resp.data
      this.initializeSelect();
    })

    this.appName = 'MELHOR FRETE'
    this.cheaper = "ain"
    this.fastest = "euin"
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    methodFoo() {
      console.log(this.appName)
    },

    initializeSelect(){
      
      var options = "<option>" + select_pHolder + "</option>";
      for(const dt of  this.api_data){
        options += "<option>"+ dt.city +"</option>";
      }
      document.getElementById("cccity").innerHTML = options;
    },

    getVal() {
      const val = document.querySelector('input').value;
      console.log(val);
    },

    analise(){

      if(this.dest_city != "")
      console.warn(this.dest_city)
      this.cheaper = "pspspspspp";
      this.fastest = "dsidujishdji";
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
  margin-top: 50px;
  size: 100px;
  font-size:larger;
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
.select-selected {
  background-color: rgb(180, 215, 251);
}

.select-selected:after {
  position: absolute;
  content: "";
  top: 14px;
  right: 10px;
  width: 0;
  height: 0;
  border: 6px solid transparent;
  border-color: rgb(0, 0, 0) transparent transparent transparent;
}

.select-selected.select-arrow-active:after {
  border-color: transparent transparent #000 transparent;
  top: 7px;
}

.select-items div,.select-selected {
  color: #000000;
  padding: 8px 16px;
  border: 1px solid transparent;
  border-color: transparent transparent rgba(0, 0, 0, 0.1) transparent;
  cursor: pointer;
}

.logo{
  height: 50px;
  padding: 10px;
}

</style>
