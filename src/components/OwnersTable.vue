<template>
<div id="owners-table">
  <table>
    <thead>
      <tr>
        <th>Firstname</th>
        <th>Lastname</th>
        <th>Email</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="owner in owners" :key="owner.id">
        <td>{{ owner.fname }}</td>
        <td>{{ owner.lname }}</td>
        <td>{{ owner.email }}</td>
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

</style>
