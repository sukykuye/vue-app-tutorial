<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <div id="name">
        <label>Employee Name</label>
        <input
            ref="first"
            v-model="employee.name"
            type="text"
            :class="{ 'has-error': submitting && invalidName }"
            @focus="clearStatus"
            @keypress="clearStatus"
        />
      </div>
      <div id="email">
        <label>Employee Email</label>
        <input
            v-model="employee.email"
            type="text"
            :class="{ 'has-error': submitting && invalidEmail }"
            @focus="clearStatus"
        />
      </div>
      <p v-if="error && submitting" class="error-message">
        Please fill out all required fields!
      </p>
      <p v-if="success" class="success-message">
        Employee successfully added.
      </p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: '',
        email: '',
      }
    }
  },
  methods: {
    handleSubmit() {
      this.submitting = true
      this.clearStatus()

      if (this.invalidName || this.invalidEmail) {
        this.error = true
        return
      }

      this.$emit('add:employee', this.employee)
      this.$refs.first.focus()
      this.employee = {
        name: '',
        email: ''
      }
      this.error = false
      this.success = true
      this.submitting = false
    },

    clearStatus(){
      this.success = false
      this.error = false
    }
  },
  computed: {
    invalidName() {
      return this.employee.name === ''
    },

    invalidEmail() {
      return this.employee.email === ''
    },
  },
}
</script>

<style>
  input[type=text] {
    width: 150px;
    padding: 12px 12px;
    margin: 8px 8px;
    box-sizing: border-box;
  }

  form {
    margin-bottom: 2rem;
  }

  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>