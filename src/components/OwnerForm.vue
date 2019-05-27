<template>
  <div id="owner-form">
    <form @submit.prevent="handleSubmit">
      <label>Firstname</label>
      <input ref="first" type="text"
        :class="{ 'has-error': submitting && invalidFName }"
        v-model="owner.fname"
        @focus="clearStatus"
        @keypress="clearStatus"
      >
      <label>Lastname</label>
      <input type="text"
        :class="{ 'has-error': submitting && invalidLName }"
        v-model="owner.lname"
        @focus="clearStatus"
        @keypress="clearStatus"
      >
      <label>Email</label>
      <input type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="owner.email"
        @focus="clearStatus"
      >
      <p
        v-if="error && submitting"
        class="error-message"
      >❗Please fill out all required fields</p>
      <p
        v-if="success"
        class="success-message"
      >✅ Owner successfully added</p>
      <button>Add Owner</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'owner-form',
  data() {
    return {
      error: false,
      submitting: false,
      success: false,
      owner: {
        fname: '',
        lname: '',
        email: '',
      }
    }
  },
  computed: {
    invalidFName() {
      return this.owner.fname === ''
    },
    invalidLName() {
      return this.owner.lname === ''
    },
    invalidEmail() {
      return this.owner.email === ''
    },
  },
  methods: {
    handleSubmit() {
      this.clearStatus()
      this.submitting = true

      if (this.invalidFName || this.invalidLName || this.invalidEmail) {
        this.error = true
        return
      }

      this.$emit('add:owner', this.owner)
      this.$refs.first.focus()
      this.owner = {
        fname: '',
        lname: '',
        email: '',
      }
      this.success = true
      this.error = false
      this.submitting = false
    },

    clearStatus() {
      this.success = false
      this.error = false
    }
  }}
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>
