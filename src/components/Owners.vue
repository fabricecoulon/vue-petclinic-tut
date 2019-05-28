<template>
  <div>
    <h1>Owners</h1>
      <!-- Retrieving events from the child component OwnerForm
      and reroute it to this application addOwner. '@' is the shortcut
      for Vue's directive 'v-on' -->
      <owner-form @add:owner="addOwner"></owner-form>
      <!-- Binding the component attribute owners to this array of owners
      ':' is the shortcut for Vue's directive 'v-bind'
      -->
      <owners-table
        :owners="owners"
        @delete:owner="deleteOwner"
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
      deleteId: -1
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
    }
  },
  mounted() {
    // This method is called once after Vue has been 'mounted' to a given DOM
    // which is specified in main.js with function mount()
    //alert("Vue has been mounted on #app")
  }
}
</script>

<style scoped>

</style>
