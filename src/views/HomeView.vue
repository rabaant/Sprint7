<template>
  <div>
   
    <HomeComp class=" text-left mx-auto"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HomeComp from '@/components/HomeComp.vue'

export default {
  name: 'HomeView',
  components: {
    HomeComp
  }
}
</script>
