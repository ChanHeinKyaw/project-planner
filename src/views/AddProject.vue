<template>
  <h1>Add Project</h1>
  <form @submit.prevent="addProject">
    <label>Project Title</label>
    <input type="text" v-model="title" />
    <span class="errMessage" @show="titleError">{{ titleError }}</span>
    <label>Project Detail</label>
    <input type="text" v-model="detail" />
    <span class="errMessage" @show="detailError">{{ detailError }}</span>
    <label>Choose Date</label>
    <Datepicker v-model="date" placeholder="Date"></Datepicker>
    <span class="errMessage" @show="dateError">{{ dateError }}</span>
    <button>Add Project</button>
  </form>
</template>

<script>
export default {
  data() {
    // const formatDate = () => {
    //   const day = new Date().getDate();
    //   const month = new Date().getMonth() + 1;
    //   const year = new Date().getFullYear();
    //   return `${day}/${month}/${year}`;
    // };
    return {
      title: "",
      detail: "",
      projects: [],
      titleError: "",
      detailError: "",
      dateError: "",
      date: "",
    };
  },
  mounted() {
    if (localStorage.getItem("projects")) {
      try {
        this.projects = JSON.parse(localStorage.getItem("projects"));
      } catch (e) {
        localStorage.removeItem("projects");
      }
    }
  },
  methods: {
    addProject() {
      if (
        this.title == "" ||
        this.detail == "" ||
        this.date == "" ||
        this.date == "Invalid date" ||
        this.date == null
      ) {
        if (this.title == "") {
          this.titleError = "The title field is required!";
        } else {
          this.titleError = "";
        }
        if (this.detail == "") {
          this.detailError = "The detail field is required!";
        } else {
          this.detailError = "";
        }
        if (
          this.date == "" ||
          this.date == "Invalid date" ||
          this.date == null
        ) {
          this.dateError = "The date field is required!";
        } else {
          this.dateError = "";
        }
        return;
      }
      this.saveProjects();
    },
    saveProjects() {
      this.projects.push({
        id: Date.now(),
        title: this.title,
        detail: this.detail,
        date: this.date,
        complete: false,
      });
      localStorage.setItem("projects", JSON.stringify(this.projects));
      this.$router.push("/");
      window.toaster.success("Project Created Success!", {
        position: "top-right",
        duration: 3000,
      });
    },
  },
};
</script>

<style>
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
}
label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}
textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
}
form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
}
.errMessage {
  color: crimson;
}
.dp__action {
  padding: 0px !important;
}

.dp__button {
  display: none !important;
}

.dp__input {
  border: none !important;
  border-bottom: 1px solid #ddd !important;
}
</style>
