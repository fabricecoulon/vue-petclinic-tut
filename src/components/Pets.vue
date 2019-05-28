<template>
  <div>
    <h1>Pets</h1>
      <pets-table :pets="pets"></pets-table>
  </div>
</template>

<script>
import PetsTable from '@/components/PetsTable.vue'

export default {
  name: 'pets',
  components: {
    PetsTable,
  },
  data() {
    return {
      pets: [
        { id:0, name: "pet-name1", ownerId: 0 },
        { id:1, name: "pet-name2", ownerId: 1 }
      ]
    }
  },
  methods: {
    addPet(pet) {
      const nbPets = this.pets.length;
      const lastId = (nbPets > 0) ? this.pets[nbPets-1].id : 0;
      const id = lastId + 1;
      const newPet = { ...pet, id };
      this.pets = [ ...this.pets, newPet ];
    }
  },
  mounted() {
  }
}
</script>

<style scoped>

</style>
