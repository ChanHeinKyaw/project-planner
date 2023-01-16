<template>
  <h1>Edit Project</h1>
  <form @submit.prevent="updateProject">
    <label>Project Title</label>
    <input type="text" v-model="title" />
    <label>Project Detail</label>
    <input type="text" v-model="detail" />
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      detail: "",
      projects: [],
    };
  },
  methods: {
    updateProject() {
      this.update();
    },
    update() {
      let data = this.projects.find((project) => {
        return project.id == this.id;
      });

      data.title = this.title;
      data.detail = this.detail;

      localStorage.setItem("projects", JSON.stringify(this.projects));
      this.$router.push("/");
    },
  },
  mounted() {
    let datas = JSON.parse(localStorage.getItem("projects"));
    datas.forEach((data) => {
      if (data.id == this.id) {
        this.title = data.title;
        this.detail = data.detail;
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
