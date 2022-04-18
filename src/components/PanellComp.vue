<template>
  <div >
    <b-form>
        <div>
            <label class="d-inline-block" for="pag">Número de páginas</label>
              <b-button class="d-inline-block bg-warning" @click="numPag--">-</b-button>
              <b-form-input class="d-inline-block text-center" type="text" name="pag" v-model="numPag"></b-form-input>
              <b-button class="d-inline-block bg-warning" @click="numPag++">+</b-button> 
              <b-icon-info v-b-modal.modal-1 class="h4 mb-0 cursor" variant="secondary"></b-icon-info>

              <b-modal id="modal-1" hide-footer hide-header>
                <p class="my-2 text-center">Selecciona el número de páginas que tendrá tu sitio web</p>
              </b-modal>          
        </div>
        <div>
            <label class="d-inline-block" for="lang">Número de idiomas</label>         
                <b-button class="d-inline-block bg-warning" @click="numLang--">-</b-button>
                <b-form-input class="d-inline-block text-center " type="text" name="lang" v-model="numLang"></b-form-input> 
                <b-button class="d-inline-block bg-warning" @click="numLang++">+</b-button>        
                <b-icon-info v-b-modal.modal-2 class="h4 mb-0 cursor" variant="secondary"></b-icon-info>

                <b-modal id="modal-2" hide-footer hide-header>
                  <p class="my-2 text-center">Selecciona el número de idiomas que tendrá tu sitio web</p>
                </b-modal>                
        </div>   
           <p>Extras:{{suma}}</p>
    </b-form>
    
  </div>
</template>

<script>
export default {
  name: 'PanellComp',
  data(){
      return {
          numPag:1,
          numLang:1,
      }
  },
  computed:{
      suma: function(){
        //calculamos los extras
         let suma = parseInt(this.numPag)*parseInt(this.numLang)*30;

        //si sólo hay una página y un idioma los extras están incluidos en el precio
         if(this.numPag ==1 && this.numLang==1)
          suma=0;
        //mandamos el dato que recogeremos en HomeComp
         this.$emit('sumPagLang',suma,this.numPag,this.numLang);
         return suma;           
      }    
  },
  watch:{
    //evitamos que el número de páginas e idiomas pueda ser inferior a 1
    numPag(x){
      x < 1 ? this.numPag = 1 : this.numPag = this.numPag
       
    },
    numLang(y){
      y < 1 ? this.numLang = 1 : this.numLang  =this.numLang
    }
    
  }

}

</script>

<style scoped>

input[type=text]{
    width: 4rem;
}
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

.cursor{
  cursor: pointer;
}
</style>
