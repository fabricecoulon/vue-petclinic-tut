<template>
  <div>
    <h1>Owners</h1>
      <span>
        <button v-show="!showAddForm" @click="onOwnerAdd">Add</button>
        <button v-show="!showAddForm" class="muted-button">Find</button>
        <button v-show="!showAddForm" class="muted-button">Schedule</button>
      </span>
      <!-- Retrieving events from the child component OwnerForm
      and reroute it to this application addOwner. '@' is the shortcut
      for Vue's directive 'v-on' -->
      <owner-form
        v-show="showAddForm"
        :owner="owner"
        @add:owner="addOwner"
        @add:cancel="showAddForm = false"
      >Edit owner
      </owner-form>
      <!-- Binding the component attribute owners to this array of owners
      ':' is the shortcut for Vue's directive 'v-bind'
      -->
      <owners-table
        :owners="owners"
        @delete:owner="deleteOwner"
        @edit:owner="editOwner"
        @save:owner="saveOwner"
        @details:select="selectOwner"
      >
      </owners-table>

      <modal v-if="showModal" @ok="onActionOk" @cancel="onActionCancel">
        <h3 slot="header">Confirm deletion</h3>
        <span slot="body">
          Are you sure you want to delete '{{ownerMatching(deleteId).firstName + " " + ownerMatching(deleteId).lastName}}'?
        </span>
      </modal>
  </div>
</template>

<script>
import OwnersTable from '@/components/OwnersTable.vue'
import OwnerForm from '@/components/OwnerForm.vue'
import Modal from '@/components/Modal.vue'

export default {
  name: 'owners',
  components: {
    OwnersTable,
    OwnerForm,
    Modal
  },
  data() {
    return {
      /*owners: [
        { id:0, fname: "fname1", lname: "lname1",
          email: "fname1.lname1@email.com", address: 'addr1', city: 'city1',
          telephone: '12345' },
        { id:1, fname: "fname2", lname: "lname2",
          email: "fname2.lname2@email.com", address: 'addr2', city: 'city2',
          telephone: '67890' }
      ],*/
      owners: [],
      showModal: false,
      deleteId: -1,
      showAddForm: false,
      owner: {}
    }
  },
  methods: {
    async getOwners() {
      try {
        const response = await fetch('http://localhost:9966/petclinic/api/owners')
        const data = await response.json()
        this.owners = data
      } catch (error) {
        console.error(error)
      }
    },
    onOwnerAdd() {
      this.showAddForm = true;
      this.owner =  {}
    },
    addOwner(owner) {
      const nbOwners = this.owners.length;
      const lastId = (nbOwners > 0) ? this.owners[nbOwners-1].id : 0;
      const id = lastId + 1;
      const newOwner = { ...owner, id };
      this.owners = [ ...this.owners, newOwner ];
    },
    deleteOwner(ownerId) {
      console.log("ownerId = ", ownerId);
      this.deleteId = ownerId;
      this.showModal = true;
    },
    doDeleteOwner(ownerId) {
      if (ownerId < 0) return;
      this.owners = this.owners.filter(
        owner => owner.id !== ownerId
      );
    },
    onActionOk() {
      this.showModal = false;
      this.doDeleteOwner(this.deleteId);
    },
    onActionCancel() {
      this.showModal = false;
      this.deleteId = -1;
    },
    ownerMatching(id) {
      const set = this.owners.filter(owner => owner.id == id);
      return (set && set.length > 0)?set[0]:null;
    },
    editOwner(id) {
      // Just some code to show how to catch events from the child OwnerTable
      const set = this.owners.filter(owner => owner.id == id);
      if (set === null) return;
    },
    saveOwner(id, updatedOwner) {
      let owner = this.ownerMatching(id);
      if (updatedOwner.firstName === '' || updatedOwner.lastName === '' || updatedOwner.telephone === '' ) {
        return;
      }
      owner.firstName = updatedOwner.firstName;
      owner.lastName = updatedOwner.lastName;
      owner.telephone = updatedOwner.telephone;
    },
    selectOwner(id) {
      console.log("selectedOwner: " + id);
      this.owner = this.ownerMatching(id);
      this.showAddForm = true;
    }
  },
  mounted() {
    //alert("mounted");
    this.getOwners();
  }
}
</script>

<style scoped>
button {
  margin: 0 0.5rem 0.5rem 0;
}
</style>
