<template>
  <div id="student-form">
    <form v-on:submit.prevent="handleSubmit">
      <!-- <label>Student Name</label> -->
      <table>
        <tr v-if="op===1" key="1">
          <td>
            <input
              placeholder="姓名"
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidName }"
              v-model="student.name"
              v-on:focus="clearStatus"
              v-on:keypress="clearStatus"
            />
          </td>
          <td>
            <input
              placeholder="性别"
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidSex }"
              v-model="student.sex"
              v-on:focus="clearStatus"
            />
          </td>
          <td>
            <input
              placeholder="生日"
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidBirthday }"
              v-model="student.birthday"
              v-on:focus="clearStatus"
              v-on:keypress="clearStatus"
            />
          </td>
        </tr>
        <tr v-else key="2">
          <td>
            <input
              placeholder="籍贯"
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidBirthplace }"
              v-model="student.birthplace"
              v-on:focus="clearStatus"
              v-on:keypress="clearStatus"
            />
          </td>
          <td>
            <input
              placeholder="系别"
              type="text"
              v-bind:class="{ 'has-error': submitting && invalidDept }"
              v-model="student.dept"
              v-on:focus="clearStatus"
              v-on:keypress="clearStatus"
            />
          </td>
        </tr>
        <tr key="3">
          <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
          <p v-if="success" class="success-message">✅ Student successfully added</p>

        </tr>
        <tr key="4">
          <td v-if="op!==0">
            <input value=""
              placeholder="学号"
              type="text"              
              v-model="id"
              v-on:focus="clearStatus"
              v-on:keypress="clearStatus"
            />
          </td>   
          <p v-else></p>       
          <td>
            <button>确定</button>
          </td>
          <p>id:{{id}},op in form:{{op}}</p>
        </tr>
      </table>
     
    </form>
  </div>
</template>

<script>
export default {
  name: "student-form",
  props: {
    op: Number
  },
  computed: {
    needId() {
      return this.op !== 0&& this.op !== 1;
    },

    needForm() {     
      return this.op===1;
    },

    invalidName() {
      return this.student.name === "";
    },

    invalidSex() {
      return this.student.sex === "";
    },

    invalidBirthday() {
      return this.student.birthday === "";
    },

    invalidBirthplace() {
      return this.student.birthplace === "";
    },

    invalidDept() {
      return this.student.dept === "";
    }
  },
  data() {
    return {
      id: null,
      tmp:null,
      submitting: false,
      error: false,
      success: false,
      student: {
        name: "",
        sex: "",
        birthday: "",
        birthplace: "",
        dept: ""
      }
    };
  },
  methods: {
    render(){
      this.data=Object.assign({}, this.id);
      this.$forceUpdate();
      console.log("form render");
    },

    handleSubmit() {
      this.submitting = true;
      this.clearStatus();
      // this.tmp=this.op;
      // this.data=Object.assign(this.tmp, this.op);
      // this.$forceUpdate();
      if (
        this.invalidName ||
        this.invalidSex ||
        this.invalidBirthday ||
        this.invalidBirthplace ||
        this.invalidDept
      ) {
        this.error = true;
        return;
      }

      this.$emit("add:student", this.student);
      this.student = {
        name: "",
        sex: "",
        birthday: "",
        birthplace: "",
        dept: ""
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