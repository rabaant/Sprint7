<template>
  <div >
    <h1>Bienvenido a la calculadora de presupuestos</h1>
    <p>Elije el servicio que necesitas, página web, consultoría SEO o campaña de Google Ads.</p>
    <p>En el caso de la página Web puedes elegir el número de páginas y los idiomas.</p>
    <b-button><router-link :to="{name:'calculadora'}">Empezamos</router-link></b-button>
  </div>
</template>

<style scoped>
a {
  text-decoration: none;
  color: white;
}
button{
  background-color: #42b983;
}
</style>
