<template>
  <div >
    <ul v-for="(budget,index) in budgets" :key="index">
      <li>Nombre Cliente:{{budget.clientName}}, Nombre Presupuesto: {{budget.budgetName}}, Total Presupuesto: {{budget.total}}
        <ul>Servicios contratados:
          <li v-if="budget.web==true">Página web (con {{budget.pag}} pag/s y {{budget.lang}} idioma/s)</li>
          <li v-if="budget.seo==true">Consultoría SEO</li>
          <li v-if="budget.sem==true">Campaña de Google Ads</li>
        </ul>
      </li>

    </ul>
    
  </div>
</template>

<script>
export default {
  name: 'ListComp',
  props:["budgets"],
     
}

</script>

<style scoped>
li{
  text-align: left;
}

</style>
