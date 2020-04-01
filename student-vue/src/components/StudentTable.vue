<template>
  <div id="student-table">
    <p v-if="students.length < 1" class="empty-table">No students</p>
    <table v-else>
      <thead>
        <tr>
          <th>Student name</th>
          <th>Student sex</th>
          <th>Actions</th>
        </tr>
        <p>op:{{op}}</p>
      </thead>
      <tbody>
        <tr v-for="student in students" v-bind:key="student.id">
          <td v-if="editing === student.id">
            <input type="text" v-model="student.name" />
          </td>
          <td v-else>{{student.name}}</td>
          <td v-if="editing === student.id">
            <input type="text" v-model="student.sex" />
          </td>
          <td v-else>{{student.sex}}</td>
          <td v-if="editing === student.id">
            <button v-on:click="editStudent(student)">Save</button>
            <button class="muted-button" v-on:click="cancelEdit(student)">Cancel</button>
          </td>
          <td v-else>
            <button v-on:click="editMode(student)">Edit</button>
            <button v-on:click="$emit('delete:student', student.id)">Delete</button>
          </td>
        </tr>   
           
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "student-table",
  props: {
    op:Number,
    students: Array
  },
  data() {
    return {
      editing: null
    };
  },
  methods: {
    editMode(student) {
      this.cachedStudent = Object.assign({}, student);
      this.editing = student.id;
      console.log("edit mode");
    },
    cancelEdit(student) {
      // student;
      Object.assign(student, this.cachedStudent);
      this.editing = null;
    },
    editStudent(student) {
      if (student.name === "" || student.sex === "") return;
      this.$emit("edit:student", student.id, student);
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