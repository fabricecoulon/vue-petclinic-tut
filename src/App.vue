<template>
  <div id="app" class="small-container">
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
      <h1>Pet owners</h1>
      <!-- Retrieving events from the child component OwnerForm
      and reroute it to this application addOwner. '@' is the shortcut
      for Vue's directive 'v-on' -->
      <owner-form @add:owner="addOwner"></owner-form>
      <!-- Binding the component attribute owners to this array of owners
      ':' is the shortcut for Vue's directive 'v-bind'
      -->
      <owners-table :owners="owners"></owners-table>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import OwnersTable from '@/components/OwnersTable.vue'
import OwnerForm from '@/components/OwnerForm.vue'

export default {
  name: 'app',
  components: {
    //HelloWorld
    OwnersTable,
    OwnerForm
  },
  data() {
    return {
      owners: [
        { id:1, fname: "fname1", lname: "lname1", email: "fname1.lname1@email.com" },
        { id:2, fname: "fname2", lname: "lname2", email: "fname2.lname2@email.com" }
      ]
    }
  },
  methods: {
    addOwner(owner) {
      const nbOwners = this.owners.length;
      const lastId = (nbOwners > 0) ? this.owners[nbOwners-1].id : 0;
      const id = lastId + 1;
      const newOwner = { ...owner, id };
      this.owners = [ ...this.owners, newOwner ];
    }
  },
  mounted() {
    // This method is called once after Vue has been 'mounted' to a given DOM
    // which is specified in main.js with function mount()
    //alert("Vue has been mounted on #app")
  }
}
</script>

<style>
.small-container {
  max-width: 680px;
}
</style>
