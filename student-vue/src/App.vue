<template>
  <div id="app" class="small-container">
    <h1>Students</h1>
    <!-- <select v-model="op" @change="updateChildren()"> -->
    <select v-model="op" @change="updateChildren()">
      <option value="0">请选择操作</option>
      <option value="1">插入</option>
      <option value="2">查询</option>
      <option value="3">删除</option>
      <option value="4">修改</option>
    </select>
    <p>{{op}}</p>
    <student-form ref="form"
      v-bind:op="op"
      v-on:add:student="addStudent" 
    />
    <student-table  ref="table"
      v-bind:op="op"
      v-bind:students="students"
      v-on:delete:student="deleteStudent"
      v-on:edit:student="editStudent"
    />
  </div>
</template>

<script>
import StudentTable from "./components/StudentTable.vue";
import StudentForm from "./components/StudentForm.vue";

export default {
  name: "App",
  components: {
    StudentTable,
    StudentForm
  },
  data() {
    return {
      op:0,
      students: [
        {
          id: 1,
          name: "Richard Hendricks",
          sex: "男",
          birthday: "",
          birthplace: "",
          dept: ""
        },
        {
          id: 2,
          name: "Bertram Gilfoyle",
          sex: "男",
          birthday: "",
          birthplace: "",
          dept: ""
        }
      ]
    };
  },

  mounted() {
    this.getStudents();
  },

  methods: {
    updateChildren(){
      // this.$refs.form.render();
      this.$refs.form.render();
      console.log("update child");
      // this.$refs.StudentTable.$forceUpdate();
    },

    editStudent(id, updatedStudent) {
      this.students = this.students.map(student =>
        student.id === id ? updatedStudent : student
      );
      
    },
    deleteStudent(id) {
      this.students = this.students.filter(student => student.id !== id);
    },
    addStudent(student) {
      const lastId =
        this.students.length > 0
          ? this.students[this.students.length - 1].id
          : 0;
      const id = lastId + 1;
      const newStudent = { ...student, id };

      this.students = [...this.students, newStudent];
    },

    async getStudents() {
      try {
        const response = await fetch("http://localhost:8083/students");
        const data = await response.json();
        this.students = data._embedded.students;
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
