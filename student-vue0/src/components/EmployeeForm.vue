<template>
  <div id="employee-form">
    <form v-on:submit.prevent="handleSubmit">
      <label>Employee name</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        v-on:focus="clearStatus"
        v-on:keypress="clearStatus"
      />
      <label>Employee Role</label>
      <input
        type="text"
        v-bind:class="{ 'has-error': submitting && invalidRole }"
        v-model="employee.role"
        v-on:focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
      <p v-if="success" class="success-message">✅ Employee successfully added</p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  computed: {
    invalidName() {
      return this.employee.name === "";
    },

    invalidRole() {
      return this.employee.role === "";
    }
  },
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      employee: {
        name: "",
        role: ""
      }
    };
  },
  methods: {
    handleSubmit() {
      this.submitting = true;
      this.clearStatus();

      if (this.invalidName || this.invalidRole) {
        this.error = true;
        return;
      }

      this.$emit("add:employee", this.employee);
      this.employee = {
        name: "",
        role: ""
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    }
  }
};
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