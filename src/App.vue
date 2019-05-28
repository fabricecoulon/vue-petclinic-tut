<template>
  <div id="app" class="small-container">
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
      <main-tabs :maintabs="maintabs"></main-tabs>
      <span v-if="current==0"><owners></owners></span>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import MainTabs from '@/components/MainTabs.vue'
import Owners from '@/components/Owners.vue'

export default {
  name: 'app',
  components: {
    //HelloWorld
    MainTabs,
    Owners
  },
  data() {
    return {
      maintabs: [
        {name: "Owners", selected: true },
        {name: "Pets", selected: false}
      ],
      current: 0
    }
  }
}
</script>

<style>
.small-container {
  max-width: 680px;
}
</style>
