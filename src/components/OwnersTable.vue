<template>
<div id="owners-table">
  <p v-if="owners.length == 0" class="empty-table">
    No owners found, table is empty.
  </p>
  <table v-else class="striped-table">
    <thead>
      <tr>
        <th>Firstname</th>
        <th>Lastname</th>
        <th>Email</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="owner in owners" :key="owner.id">
        <td v-if="owner.id != editId">
          {{ owner.fname }}
        </td>
        <td v-else>
          <input type="text" v-model="tmpOwner.fname" />
        </td>
        <td v-if="owner.id != editId">
          {{ owner.lname }}
        </td>
        <td v-else>
          <input type="text" v-model="tmpOwner.lname" />
        </td>
        <td v-if="owner.id != editId">
          {{ owner.email }}
        </td>
        <td v-else>
          <input type="text" v-model="tmpOwner.email" />
        </td>
        <td v-if="owner.id != editId">
          <!-- one can add the emit directly in the v-on:click directive too
          -->
          <button @click="toggelEditMode(owner.id, owner)">Edit</button>
          <button @click="$emit('delete:owner', owner.id)">Delete</button>
        </td>
        <td v-else>
          <button @click="saveOwner(owner.id, owner)">Save</button>
          <button @click="toggleReadMode()">Cancel</button>
        </td>
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
export default {
  name: 'owners-table',
  props: {
    owners: Array
  },
  methods: {
    toggelEditMode(id, owner) {
      this.editId = id;
      this.tmpOwner.fname = owner.fname;
      this.tmpOwner.lname = owner.lname;
      this.tmpOwner.email = owner.email;
      this.$emit('edit:owner', owner.id);
    },
    toggleReadMode() {
      this.editId = -1;
    },
    saveOwner(id, owner) {
      // Remember that the owner is immutable, the data is bound to the parent
      // Owners. All 'props' form a one-way-down binding to prevent the child
      // accidentally mutating the parent's state and therefore, one way to
      // change the data is by notifying the parent
      this.$emit('save:owner', owner.id, this.tmpOwner);
      this.toggleReadMode();
    }
  },
  data() {
    return {
      editId: -1,
      tmpOwner: { fname: '', lname: '', email: '' }
    }
  }
}
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>
