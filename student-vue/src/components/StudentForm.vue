<template>
  <div idTmp="student-form">
    <form v-on:submit.prevent="handleVerify">
      <!-- <label>Student Name</label> -->
      <table v-if="op!=='0'">
        <div v-if="op==='1'">
          <tr>
            <td>
              <input
                placeholder="Name"
                type="text"
                v-bind:class="{ 'has-error': submitting && invalidName }"
                v-model="student.name"
                v-on:focus="clearStatus"
                v-on:keypress="clearStatus"
              />
            </td>
            <td>
              <input
                placeholder="Sex"
                type="text"
                v-bind:class="{ 'has-error': submitting && invalidSex }"
                v-model="student.sex"
                v-on:focus="clearStatus"
              />
            </td>
            <td>
              <input
                placeholder="Birthday"
                type="text"
                v-bind:class="{ 'has-error': submitting && invalidBirthday }"
                v-model="student.birthday"
                v-on:focus="clearStatus"
                v-on:keypress="clearStatus"
              />
            </td>
          </tr>
          <tr>
            <td>
              <input
                placeholder="BirthPlace"
                type="text"
                v-bind:class="{ 'has-error': submitting && invalidBirthPlace }"
                v-model="student.birthPlace"
                v-on:focus="clearStatus"
                v-on:keypress="clearStatus"
              />
            </td>
            <td>
              <input
                placeholder="Department"
                type="text"
                v-bind:class="{ 'has-error': submitting && invalidDept }"
                v-model="student.dept"
                v-on:focus="clearStatus"
                v-on:keypress="clearStatus"
              />
            </td>
          </tr>
          <tr>
            <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
            <p v-if="success" class="success-message">✅ Student successfully added</p>
          </tr>
        </div>

        <tr>
          <td v-if="op!=='1'">
            <input
              value
              placeholder="Sno"
              type="text"
              v-model="idTmp"
              v-on:focus="clearStatus"
              v-on:keypress="clearStatus"
            />
          </td>
          <td>
            <button>Verify</button>
          </td>
          <p>idTmp:{{idTmp}},op in form:{{op}}</p>
        </tr>
      </table>
    </form>
  </div>
</template>

<script>
export default {
  name: "student-form",
  props: {
    op: String
  },
  computed: {
    // needId() {
    //   return this.op !== 0&& this.op !== 1;
    // },

    // needForm() {
    //   return this.op===1;
    // },

    invalidName() {
      return this.student.name === "";
    },

    invalidSex() {
      return this.student.sex === "";
    },

    invalidBirthday() {
      return this.student.birthday === "";
    },

    invalidBirthPlace() {
      return this.student.birthPlace === "";
    },

    invalidDept() {
      return this.student.dept === "";
    }
  },
  data() {
    return {
      idTmp: "",
      submitting: false,
      error: false,
      success: false,
      student: {
        name: "",
        sex: "",
        birthday: "",
        birthPlace: "",
        dept: ""
      }
    };
  },
  methods: {
    render(x) {
      // this.data=Object.assign({}, this.idTmp);
      this.op = x;
      this.$forceUpdate();
      console.log("form render,op:" + this.op);
    },

    handleVerify() {
      this.submitting = true;
      this.clearStatus();
      // this.tmp=this.op;
      // this.data=Object.assign(this.tmp, this.op);
      // this.$forceUpdate();
      console.info("verify");
      if (this.op === "1") {
        //insert
        if (
          this.invalidName ||
          this.invalidSex ||
          this.invalidBirthday ||
          this.invalidBirthPlace ||
          this.invalidDept
        ) {
          this.error = true;
          return;
        }
        this.$emit("add:student", this.student);

      } else {
        
        if (this.idTmp === "") this.$parent.getStudents();
        else this.$parent.getOneStudent(this.idTmp);
      }

      // console.info("verify");
      // if (this.idTmp === "") this.$parent.getStudents();
      // else this.$parent.getOneStudent(this.idTmp);

      // switch(Number(op)){
      //   case 1:
      //     this.$emit("add:student", this.student);
      //     break;
      //   case 2:
      //     this.$emit("query:")

      // }

      // if (this.op === "1") this.$emit("add:student", this.student);

      this.student = {
        name: "",
        sex: "",
        birthday: "",
        birthPlace: "",
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