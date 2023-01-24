<template>
  <h1>Edit Project</h1>
  <form @submit.prevent="updateProject">
    <label>Project Title</label>
    <input type="text" v-model="title" />
    <span class="errMessage" @show="titleError">{{ titleError }}</span>
    <label>Project Detail</label>
    <input type="text" v-model="detail" />
    <span class="errMessage" @show="detailError">{{ detailError }}</span>
    <label>Choose Date</label>
    <Datepicker v-model="date" placeholder="Date"></Datepicker>
    <span class="errMessage" @show="dateError">{{ dateError }}</span>
    <button>Update Project</button>
  </form>
</template>

<script>
import moment from "moment";
export default {
  props: ["id"],
  data() {
    // const formatDate = (date) => {
    //   const day = date.getDate();
    //   const month = date.getMonth() + 1;
    //   const year = date.getFullYear();
    //   return `${day}/${month}/${year}`;
    // };
    return {
      title: "",
      detail: "",
      date: "",
      projects: [],
      titleError: "",
      detailError: "",
      dateError: "",
    };
  },
  methods: {
    updateProject() {
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
      this.update();
    },
    update() {
      let data = this.projects.find((project) => {
        return project.id == this.id;
      });

      data.title = this.title;
      data.detail = this.detail;
      data.date = this.date;

      localStorage.setItem("projects", JSON.stringify(this.projects));
      this.$router.push("/");
      window.toaster.success("Project Updated Success!", {
        position: "top-right",
        duration: 3000,
      });
    },
  },
  mounted() {
    let datas = JSON.parse(localStorage.getItem("projects"));
    datas.forEach((data) => {
      if (data.id == this.id) {
        this.title = data.title;
        this.detail = data.detail;
        this.date = data.date;
      }
    });

    if (localStorage.getItem("projects")) {
      try {
        this.projects = JSON.parse(localStorage.getItem("projects"));
      } catch (e) {
        localStorage.removeItem("projects");
      }
    }
  },
};
</script>
