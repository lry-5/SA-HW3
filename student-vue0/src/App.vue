<template>
  <div id="app" class="small-container">
    <h1>Students</h1>
    <!-- <select v-model="op" @change="updateChildren()"> -->
    <select v-model="op" @change="clearFormStatus()">
      <option value="0">Please choose your operation</option>
      <option value="1">Insert</option>
      <option value="2">Query</option>
      <option value="3">Delete</option>
      <option value="4">Update</option>
    </select>
    <!-- <p>{{op}}</p> -->
    <student-form ref="form" v-bind:op="op" v-on:add:student="addStudent" />
    <student-table
      ref="table"
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
      op: "0",
      count: 0,
      students: Array
      
    };
  },

  mounted() {
    this.getStudents();
    // this.count=this.students.length;
  },

  methods: {   
    clearFormStatus(){
      this.$refs.form.clearStatus();
    },    
    editStudent(id, updatedStudent) {
      this.students = this.students.map(student =>
        student.id === id ? updatedStudent : student
      );
      this.editStudentBack(id, updatedStudent);
    },
    deleteStudent(id) {
      this.students = this.students.filter(student => student.id !== id);
      this.deleteStudentBack(id);
    },
    addStudent(student) {
      // console.log("add student 0");
      // this.getStudents();
      const lastId =
        this.students.length > 0
          ? this.students[this.students.length - 1].id
          : 0;
      // const lastId =this.count;
      const id = lastId + 1;
      // console.log("add student 1");
      const newStudent = { ...student, id };

      this.students = [...this.students, newStudent];
      // this.count+=1;
      console.log("add student 2");
      this.addStudentBack(newStudent);
    },

    async getStudents() {
      try {
        console.log("get students");
        const response = await fetch("http://localhost:8083/students/");
        const data = await response.json();
        this.students = data._embedded.students;
      } catch (error) {
        console.error(error);
      }
    },

    async getOneStudent(id) {
      try {
        const response = await fetch("http://localhost:8083/students/" + id);
        // console.log("get one student,response:"+response);
        const data = await response.json();
        // const data = response.object;

        // console.log("data:"+data);

        this.students = new Array(data);
        // this.students = [JSON.parse(data)];
        // this.students = data;
      } catch (error) {
        console.error(error);
      }
    },

    async addStudentBack(student) {
      try {
        const response = await fetch("http://localhost:8083/students", {
          method: "POST",
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json"
          },
          body: JSON.stringify(student)
        });
        if (response.ok) {
          console.log("add success!");
          // alert('add success!')
          // this.$router.push('/')
        } else {
          console.log(response.status);
        }
      } catch (error) {
        console.log(error);
      }
      console.log("add student 2 back2");
    },

    async deleteStudentBack(id) {
      console.log("delete back 0");
      try {
        const response = await fetch("http://localhost:8083/students/" + id, {
          method: "DELETE",
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json"
          },
          body: null
          // body: JSON.stringify(this.studentForm)
        });
        if (response.ok) {
          console.log("delete student back");
          // alert('delete success!')
          // this.$router.push('/')
        } else {
          console.log(response.status);
        }
      } catch (error) {
        console.log(error);
      }
    },

    async editStudentBack(id, student) {
      console.log("edit back 0");
      try {
        const response = await fetch("http://localhost:8083/students/" + id, {
          method: "PUT",
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json"
          },
          body: JSON.stringify(student)
        });
        if (response.ok) {
          console.log("update success!");
          // alert('add success!')
          // this.$router.push('/')
        } else {
          console.log(response.status);
        }
      } catch (error) {
        console.log(error);
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
