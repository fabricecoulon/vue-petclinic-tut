<template>
  <div>
    <h1>Owners</h1>
      <span>
        <button v-show="!showAddForm" @click="showAddForm = true">Add</button>
        <button v-show="!showAddForm" class="muted-button">Find</button>
      </span>
      <!-- Retrieving events from the child component OwnerForm
      and reroute it to this application addOwner. '@' is the shortcut
      for Vue's directive 'v-on' -->
      <owner-form
        v-show="showAddForm"
        @add:owner="addOwner"
        @add:cancel="showAddForm = false"
      >Add a new owner
      </owner-form>
      <!-- Binding the component attribute owners to this array of owners
      ':' is the shortcut for Vue's directive 'v-bind'
      -->
      <owners-table
        :owners="owners"
        @delete:owner="deleteOwner"
        @edit:owner="editOwner"
        @save:owner="saveOwner"
      >
      </owners-table>

      <modal v-if="showModal" @ok="onActionOk" @cancel="onActionCancel">
        <h3 slot="header">Confirm deletion</h3>
        <span slot="body">
          Are you sure you want to delete '{{ownerMatching(deleteId).fname + " " + ownerMatching(deleteId).lname}}'?
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
      owners: [
        { id:0, fname: "fname1", lname: "lname1", email: "fname1.lname1@email.com" },
        { id:1, fname: "fname2", lname: "lname2", email: "fname2.lname2@email.com" }
      ],
      showModal: false,
      deleteId: -1,
      showAddForm: false
    }
  },
  methods: {
    addOwner(owner) {
      const nbOwners = this.owners.length;
      const lastId = (nbOwners > 0) ? this.owners[nbOwners-1].id : 0;
      const id = lastId + 1;
      const newOwner = { ...owner, id };
      this.owners = [ ...this.owners, newOwner ];
    },
    deleteOwner(ownerId) {
      this.showModal = true;
      this.deleteId = ownerId;
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
      if (updatedOwner.fname === '' || updatedOwner.lname === '' || updatedOwner.email === '' ) {
        return;
      }
      owner.fname = updatedOwner.fname;
      owner.lname = updatedOwner.lname;
      owner.email = updatedOwner.email;
    }
  }
}
</script>

<style scoped>
button {
  margin: 0 0.5rem 0.5rem 0;
}
</style>
