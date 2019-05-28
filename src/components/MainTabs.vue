<template>
  <div id="main-tabs">
    <!-- A simple menu made of one active button and inactive or
    muted-buttons. This is the way with w-bind or simply ':class'
    to conditionnally apply a css class style based on some object props.
    -->
    <span v-for="t in maintabs" :key="t.id">
      <button
        :class="{'button': t.selected, 'muted-button': !t.selected}"
        @click="buttonClicked(t.id)"
      >
      {{ t.name }}
      </button>
    </span>
  </div>
</template>

<script>
export default {
  name: 'main-tabs',
  props: {
    maintabs: Array
  },
  methods: {
    buttonClicked(id) {
      this.$emit('select:tabid', id);
    }
  }
}
</script>

<style scoped>
button {
  margin-top: 1rem;
  margin-right: 1rem;
}
</style>
