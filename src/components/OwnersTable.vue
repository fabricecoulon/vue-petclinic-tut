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
        <td>{{ owner.fname }}</td>
        <td>{{ owner.lname }}</td>
        <td>{{ owner.email }}</td>
        <td>
          <!-- one can add the emit directly in the v-on:click directive too
          -->
          <button @click="$emit('edit:owner', owner.id)">Edit</button>
          <button @click="$emit('delete:owner', owner.id)">Delete</button>
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
  }
}
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>
