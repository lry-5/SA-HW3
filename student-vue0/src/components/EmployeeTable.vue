<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees</p>
    <table v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee role</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" v-bind:key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{employee.name}}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.role" />
          </td>
          <td v-else>{{employee.role}}</td>
          <td v-if="editing === employee.id">
            <button v-on:click="editEmployee(employee)">Save</button>
            <button class="muted-button" v-on:click="cancelEdit(employee)">Cancel</button>
          </td>
          <td v-else>
            <button v-on:click="editMode(employee)">Edit</button>
            <button v-on:click="$emit('delete:employee', employee.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    editMode(employee) {
      this.cachedEmployee = Object.assign({}, employee);
      this.editing = employee.id;
    },
    cancelEdit(employee) {
      Object.assign(employee, this.cachedEmployee);
      this.editing = null;
    },
    editEmployee(employee) {
      if (employee.name === "" || employee.role === "") return;
      this.$emit("edit:employee", employee.id, employee);
      this.editing = null;
    }
  }
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}
</style>