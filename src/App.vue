<template>
  <div id="app" class="small-container">
      <!-- In the custom event listener below do not use "selectTab(arg)"
      but just "selectTab" -->
      <main-tabs :maintabs="maintabs" @select:tabid="selectTab"></main-tabs>
      <span v-show="maintabs[0].selected"><owners></owners></span>
      <span v-show="maintabs[1].selected"><pets></pets></span>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import MainTabs from '@/components/MainTabs.vue'
import Owners from '@/components/Owners.vue'
import Pets from '@/components/Pets.vue'

export default {
  name: 'app',
  components: {
    //HelloWorld
    MainTabs,
    Owners,
    Pets
  },
  data() {
    return {
      maintabs: [
        {id: 0, name: "Owners", selected: true },
        {id: 1, name: "Pets", selected: false}
      ]
    }
  },
  methods: {
    selectTab(tabid) {
      this.maintabs.forEach(tab => {
        tab.selected = (tab.id == tabid);
      });
    }
  }
}
</script>

<style>
.small-container {
  max-width: 680px;
}
</style>
