<template>
  <div id="app">
  <vue-pithy-calendar @chooseDay="getChooseDay"/>
    
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {
  
  },
   methods: {
   getChooseDay(msg){
     console.log(msg);
   }
    
  },
}
</script>

<style lang="scss">

</style>
