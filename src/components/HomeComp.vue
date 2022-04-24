<template>
  <div>
    <b-container>
      <b-row>
        <b-col xl="4" lg="4" md="12" sm="12">
        <b-form>
          <h1>Qué quieres hacer?</h1>
          <div class="d-flex flex-column align-items-start mx-auto ps-1">
            <b-form-input type="text" v-model="clientName" placeholder="Introduce el nombre del cliente" class="mb-2"></b-form-input>
            <b-form-input type="text" v-model="budgetName" placeholder="Introduce el nombre del presupuesto" class="mb-2"></b-form-input>
            <b-form-checkbox name="web" id="web" value="500" v-model="arrTotal" @change="check()" class="ms-2">Una página web (500€)</b-form-checkbox>
            <PanellComp @sumPagLang="sumaTotal" :class="{show: show == true, hide: show==false}" :resetPagLang="resetPagLang" class="ms-2"/>
            <b-form-checkbox name="seo" id="seo" value="300" v-model="arrTotal" @change="check()" class="ms-2">Una consultoría SEO (300€)</b-form-checkbox>
            <b-form-checkbox name="sem" id="sem" value="200" v-model="arrTotal" @change="check()" class="ms-2">Una campaña de Google Ads (200€)</b-form-checkbox>        
          </div>
          <p><b>Precio:{{total}} €</b></p>
          <b-button @click="save()" class="mb-3">Guardar</b-button>
          <b-button @click="$router.push('/')" class="ms-5 mb-3">Volver</b-button>
        </b-form>
        </b-col>
        <b-col xl="8" lg="8" md="12" sm="12">
          <h2 class="mb-2">Listado de presupuestos</h2>
          <b-button @click="sortAsc(budgetList)" class="mb-2">Ordenar alfabéticamente (nombre)</b-button>
          <b-button @click="sortDate(budgetList)" class="mb-2">Ordenar por fecha</b-button>
          <b-button @click="restart(budgetList)" class="mb-2 ms-1">Reiniciar orden</b-button>
          <b-input type="text" v-model="text" v-on:keyup.enter="find(budgetList,text)" placeholder="introduce el texto a buscar como nombre del presupuesto y pulsa enter">{{text}}</b-input>
          
          <!--Si hay objetos en el array de búsqueda manda éste a ListComp y si no usa el array completo -->
          <ListComp v-if="budgetSearch.length==0" :budgets="budgetList"/>
          <ListComp v-else :budgets="budgetSearch"/>
        
       
        </b-col>
      </b-row>
     </b-container>
  </div>
</template>

<script>
import PanellComp from '@/components/PanellComp.vue'
import ListComp from '@/components/ListComp.vue'

export default {
  name: 'HomeComp',
  components:{
    PanellComp,
    ListComp
  },
  data(){
    return {
      arrTotal:[],
      sumaPanel:0,
      sumaPanelWeb:0,
      show:false,
      budgetName:'',
      clientName:'',
      budgetList:[],
      budgetSearch:[],
      web:false,
      seo:false,
      sem:false,
      numPag:1,
      numLang:1,
      text:'',
      resetPagLang:false,
     
    }
  },
  computed:{
    total : function(){ 
      //si se ha seleccionado la página web (500 está presente en el array) se suman los extras
       if (this.arrTotal.find(e => e=="500")){
         this.sumaPanelWeb=this.sumaPanel;
      }else{
        this.sumaPanelWeb=0;
      }
      return (this.arrTotal.reduce((x,y)=>parseFloat(x)+parseFloat(y),0))+this.sumaPanelWeb
    }
  },

  mounted(){
      if(localStorage.getItem('budgetList')){
            this.budgetList=JSON.parse(localStorage.getItem('budgetList'));
          }
  },

  methods:{
    //recogemos la variable que viene de PanellComp
    sumaTotal(suma,numPag,numLang){
      this.sumaPanel=suma;
      this.numPag=numPag;
      this.numLang=numLang;
    },
    //para mostrar u ocultar el PanellComp dependiendo de si se ha seleccionado la página web (el precio está en el array)
    //también guardamos un boolean para saber la opción seleccionada y guardarla en el objeto presupuesto
    check(){
      if (this.arrTotal.find(e => e=="500")){
        this.show=true;
        this.web=true;
      }else{
        this.show=false;
        this.web=false;
      }
      this.arrTotal.find(e => e=="300") ?  this.seo=true :  this.seo=false;
      this.arrTotal.find(e => e=="200") ?  this.sem=true :  this.sem=false;
    },
    //guardamos en un array el presupuesto como un objeto 
    save(){
      if(this.clientName =='' || this.budgetName==''){
        alert("Tienes que rellenar todos los campos")
      }

      if(this.web == true || this.seo == true || this.sem == true){
          if(this.web != true) {
            this.numPag=0;
            this.numLang=0;
          }
         
         this.budgetList.push({clientName: this.clientName, 
                            budgetName: this.budgetName, 
                            web: this.web, 
                            seo: this.seo, 
                            sem: this.sem,
                            pag:this.numPag,
                            lang:this.numLang,
                            total: this.total,
                            date:Date.now()});
        const parsed = JSON.stringify(this.budgetList);
        localStorage.setItem('budgetList', parsed);
      }else{
        alert("Tienes que marcar un servicio");
      }
     //para resetear el array de búsqueda y que vuelva a usar el array completo
      this.budgetSearch=[];
      this.text='';
    //para vaciar el formulario
      this.clientName='';
      this.budgetName='';
      this.arrTotal=[];
      this.resetPagLang=true;
     
    },
  //ordenar por nombre alfabéticamente
    sortAsc(budgets){
      //para resetear el array de búsqueda y que vuelva a usar el array completo
      this.budgetSearch=[];
      this.text='';
      return this.budgetList=budgets.sort((previous,next)=>
      (previous.budgetName < next.budgetName) ? -1 : 1);
      
    },
  //ordenar por fecha más reciente
    sortDate(budgets) {
      //para resetear el array de búsqueda y que vuelva a usar el array completo
      this.budgetSearch=[];
      this.text='';
      return this.budgetList=budgets.sort((previous,next)=>
      (previous.date > next.date) ? -1 : 1);
     
    },
  //orden original (fecha más antigua)
    restart(budgets){
      //para resetear el array de búsqueda y que vuelva a usar el array completo
      this.budgetSearch=[];
      this.text='';
      return this.budgetList=budgets.sort((previous,next)=>
      (previous.date > next.date) ? 1 : -1);
     
    },

  //encontrar el texto de búsqueda por nombre en el array, creo un nuevo array que contenga los objetos que coinciden con la búsqueda
    find(budgets,text){
      let result = this.budgetSearch=budgets.filter(x=>x.budgetName.includes(text));
      if (result.length==0){
        alert('No hay resultados con esta búsqueda');
        this.text='';
      }else{
         return this.budgetSearch = result;
      }
     

    }
  },
  //ejercicio 9
  /* computed:{
      url: function(){
       
         this.$emit('changeUrl',this.web,this.seo,this.sem,this.numPag,this.numLang);
                   
      }    
  },*/
 
}

</script>

<style scoped>

a {
  text-decoration: none;
  color: white;
}
button{
  background-color: #42b983;
}

a {
  color: #42b983;
}

.show{
  display:inline-block;
}

.hide{
  display:none;
}

</style>
