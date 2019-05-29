<template>
  <div id="owner-form">
    <slot></slot>
    <!-- When pressing this form button, this component
    handleSubmit will be called first: -->
    <form @submit.prevent="handleSubmit">
      <label>First Name</label>
      <!-- ':class' is Vue's class style binding
      it is used in this case to dynamically set the style class of this input
      to {'has-error': true} style if both submitting is true and
      invalidFName is true. A similar method is used for the two other inputs
      -->
      <input ref="first" type="text"
        :class="{ 'has-error': submitting && invalidFName }"
        v-model="owner.fname"
        @focus="clearStatus"
        @keypress="clearStatus"
      >
      <label>Last Name</label>
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
      <label>Address</label>
      <input type="text" />
      <label>City</label>
      <input type="text" />
      <label>Telephone</label>
      <input type="text" />
      <!-- In case of error on submitting -->
      <p
        v-if="error && submitting"
        class="error-message"
      >❗Please fill out all required fields</p>
      <p
        v-if="success"
        class="success-message"
      >✅ Owner '{{ showFullName }}' successfully added</p>
      <span>
        <button>Save</button>
        <button type=button @click="$emit('add:cancel')">Cancel</button>
      </span>
    </form>
  </div>
</template>

<script>
export default {
  name: 'owner-form',
  props: [
  ],
  data() {
    let data = {
      error: false,
      submitting: false,
      success: false,
      owner: {
        fname: '',
        lname: '',
        email: '',
      },
      lastsubmittedowner: {
        fname: '',
        lname: ''
      }
    };
    return data;
  },
  // computed properties on the forms 'v-model' this.owner...
  // these are methods that can be called before rendering occurs to process
  // data before is is shown. For example showFullName concat the last
  // submitted owners' first and last name
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
    showFullName() {
      return this.lastsubmittedowner.fname + " " + this.lastsubmittedowner.lname
    }
  },
  // Custom methods MUST be put under Vue's 'methods:'
  methods: {
    // Called by pressing the form's template button
    handleSubmit() {
      this.clearStatus()
      // Flag that we are in status 'submitting'
      this.submitting = true

      if (this.invalidFName || this.invalidLName || this.invalidEmail) {
        this.error = true
        return
      }

      // Emit an event that can be caught by any registered listeners
      // this event is by Vue standards called 'add:owner' with colon and
      // has one argument, this owner's object/model
      this.$emit('add:owner', this.owner)

      // Return the focus to the form's element tagged with ref "first"
      this.$refs.first.focus()

      this.lastsubmittedowner.fname = this.owner.fname
      this.lastsubmittedowner.lname = this.owner.lname
      this.owner = {
        fname: '',
        lname: '',
        email: '',
      }
      this.success = true
      this.error = false
      // Flag that we are not in status 'submitting' anymore
      this.submitting = false
    },

    clearStatus() {
      this.success = false
      this.error = false
    }
  }
}
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

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
