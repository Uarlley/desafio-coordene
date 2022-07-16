<template>
  <div class="title">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand class="ml-2">
        <img class="logo" src="../assets/logo.png">
        <b style='color:black'>{{ appName}}</b>
      </b-navbar-brand>
    </b-navbar>

  <main>
    <b class="top_text">{{"Como é o frete que você precisa ?"}}</b>
    <hr style="width: 350px" size="4px"> 


    <div class="content">
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
        <button v-on:click="analise" style=" background-color: rgb(147, 196, 125) !important;"> <b>Analisar</b> </button>
      </div>
    </div>
    


    <div id="anssect" class="result" >
      <b >{{"Estas são as melhores alternativas de frete que encontramos pra você."}}</b>
      <hr style="width: 650px" size="4px"> 
    
      <div >
        <p class="cheap">Frete mais barato: <b>{{ans_cheap}}</b></p>
      </div>
      <div >
        <p class="fast" >Frete mais rápido: <b>{{ans_fast}}</b></p>
      </div>

    </div>

    
  </main>

 
  </div>


</template>

<script>

var select_pHolder = "Selecione aqui o destino do frete"
var cities = []

//var weight_pHolder = "Insira aqui o peso da carga em Kg"
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
    const dest_city = select_pHolder;
    const ans_cheap = '';
    const ans_fast = '';

    return {
      appName,
      cheaper,
      fastest,
      api_data:undefined,
      weight,
      dest_city,
      ans_cheap,
      ans_fast
      
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
    //document.getElementById("anssect").style.display = "none";
    
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    methodFoo() {
      console.log(this.appName)
    },

    initializeSelect(){
      for(const dt of  this.api_data){
        if(cities.find(element => element === dt.city) === undefined) cities.push(dt.city)
      }
      var options = "<option>" + select_pHolder + "</option>";
      for(const city of  cities) options += "<option>"+ city +"</option>";
      
      document.getElementById("cccity").innerHTML = options;
    },

    getVal() {
      const val = document.querySelector('input').value;
      console.log(val);
    },

    getShippingTime(company){
    
      return parseInt(company.lead_time.slice(0,company.lead_time.length-1))
    },

    
    //comparação dos objetos na ordenação
    compare(obj1, obj2, criteria){
      if(criteria === 1){
        if(this.getPrice(obj1) === this.getPrice(obj2))
          return this.getShippingTime(obj1) > this.getShippingTime(obj2)
        else return this.getPrice(obj1) > this.getPrice(obj2)
      }
      else{
        if(this.getShippingTime(obj1) === this.getShippingTime(obj2))
          return this.getPrice(obj1) > this.getPrice(obj2)
        else return this.getShippingTime(obj1) > this.getShippingTime(obj2)
      }
    },

    //Ordena a lista de objetos a partir de um determinado critério
    Sort(companies, criteria){
      var tmp
      var length = companies.length;  
      for (var i = 0; i < length; i++) { 
        for (var j = 0; j < (length - i - 1); j++) {
          if(this.compare(companies[j], companies[j+1], criteria)) {
            tmp = companies[j]; 
            companies[j] = companies[j+1]; 
            companies[j+1] = tmp; 
          }
        }
      }
  },
    getPrice(company){
        var cost
        if(this.weight <= 100) cost = parseFloat(company.cost_transport_light.slice(2))
        else cost = parseFloat(company.cost_transport_heavy.slice(2))
        return cost*this.weight;
    },
    analise(){
          
      if(this.dest_city != select_pHolder && this.weight != ''){
        
        var companies_objs = []
        var obj

        for(obj of this.api_data) if(obj.city === this.dest_city) companies_objs.push(obj).lengh

        //a partir dessa ordenação, se tempo de entrega de duas empresas for o mesmo
        //sera escolhida a de menor custo
        this.Sort(companies_objs, 0)
        this.fastest = companies_objs[0]
        
        //Analogamente, se custo de duas empresas for o mesmo
        //sera escolhida a de menor tempo
        this.Sort(companies_objs, 1)
        this.cheaper = companies_objs[0]

        this.ans_cheap = "Transportadora " + this.cheaper.name + " - R$ " + Number(this.getPrice(this.cheaper)).toFixed(2) + " - " + this.cheaper.lead_time
        this.ans_fast = "Transportadora " + this.fastest.name + " - R$ " + Number(this.getPrice(this.fastest)).toFixed(2) + " - " + this.fastest.lead_time

        document.getElementById("anssect").style.display = "flex";
      }
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
  margin-top: 5vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title .navbar-brand {
  margin-left: 20px;
}
.Destino{
  size: 100px;
  font-size: larger;
}

.Peso{
  margin-top: 10px;
  font-size: larger;
}
input{
  background-color: rgb(180, 215, 251);
  border: 6px solid transparent;
  width: 350px !important;
  height: 45px;
}

.Botao{
  margin-top: 50px;
  height: fit-content;
  width: fit-content;
  display: flex;
  margin-left: 125px;

}
.Botao button{
  padding: 10px;
  border-radius: 10px;
}
.select-selected {
  background-color: rgb(180, 215, 251);
  width: 350px !important;
  height: 45px;
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

.result{
  margin-top: 20px;
  display: none;
  font-size: larger;
  padding-left: 3vw!important;
  flex-direction: column;
  padding-right: 3vw;
  max-width: 100vw;
  align-items: center;
}

.top_text{
  font-size: larger;
}

.fast{
  background-color: rgb(180, 215, 251);
  width: fit-content;
}

.cheap{
  background-color: rgb(147, 196, 125);
  width: fit-content;
  
}
.result p{
  outline-width: 2px;
  outline-color: #000;
  outline-style: dashed;
  border-radius: 10px;
  padding: 10px;
  margin-top: 10px;
  height: fit-content;
  
  
}

</style>S
