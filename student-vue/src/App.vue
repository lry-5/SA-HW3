<template>
  <div id="app" class="small-container">
    <h1>Students</h1>
    <!-- <select v-model="op" @change="updateChildren()"> -->
    <select v-model="op">
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
      students: Array
      // students: [
      //   {
      //     id: 1,
      //     name: "Richard Hendricks",
      //     sex: "男",
      //     birthday: "",
      //     birthPlace: "",
      //     dept: ""
      //   },
      //   {
      //     id: 2,
      //     name: "Bertram Gilfoyle",
      //     sex: "男",
      //     birthday: "",
      //     birthPlace: "",
      //     dept: ""
      //   }
      // ]
    };
  },

  mounted() {
    this.getStudents();
  },

  methods: {
    // updateChildren(){
    //   // this.$refs.form.render();
    //   // this.$refs.form.render(this.op);
    //   // console.log("update child");
    //   // this.$refs.StudentTable.$forceUpdate();
    // },

    editStudent(id, updatedStudent) {
      this.students = this.students.map(student =>
        student.id === id ? updatedStudent : student
      );
    },
    deleteStudent(id) {
      this.students = this.students.filter(student => student.id !== id);
    },
    addStudent(student) {
      console.log("add student 0");
      const lastId =
        this.students.length > 0
          ? this.students[this.students.length - 1].id
          : 0;
      const id = lastId + 1;
      console.log("add student 1");
      const newStudent = { ...student, id };

      this.students = [...this.students, newStudent];
      console.log("add student 2");
      this.addStudentBack(newStudent);
    },

    async getStudents() {
      try {
        console.log("get students");
        const response = await fetch("http://localhost:8083/students");
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

    addStudentBack(student) {
      // try {
      //   // const response =await fetch("http://localhost:8083/students/");
      //   // console.log("get one student,response:"+response);
      //   // const data =await response.json();
      //   // const data = response.object;
      //   // console.log("data:"+data);
      //   // this.students = new Array(data);
      //   // this.students = [JSON.parse(data)];
      //   // this.students = data;
      // } catch (error) {
      //   console.error(error);
      // }

      // this.btnSaveLoading = true;
      // // let request;
      // console.log("add student 2 back");
      // let request = this.$http.post('http://localhost:8083/students/', student);
      // request
      //   .then(
      //     data => {
      //       this.dialogEditVisible = false;
      //       console.log(data);
      //       console.log(data.data);
      //       this.$layer_message(data.data.data, "error");
      //       this.fetch();
      //     },
      //     res => this.$layer_message(res.result)
      //   )
      //   .finally(() => (this.btnSaveLoading = false));
      console.log("add student 2 back0");
      this.axios
        .post("http://localhost:8083/students/", JSON.stringify(student), {
          headers: {
            "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8"
          }
        })
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          console.log(error);
        });
      console.log("add student 2 back2");
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
