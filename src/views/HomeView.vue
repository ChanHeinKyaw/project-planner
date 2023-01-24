<template>
  <div class="home">
    <h1>Home</h1>
    <FilterNav @filterValue="current = $event" :current="current"></FilterNav>
    <div v-for="project in filteredProjects" :key="project.id">
      <SingleProject
        :project="project"
        @delete="deleteProject"
        @complete="completeProject"
        @removeProject="removeProject"
      ></SingleProject>
    </div>
  </div>
</template>

<script>
import FilterNav from "../components/FilterNav";
import SingleProject from "../components/SingleProject";
import moment from "moment";
export default {
  name: "HomeView",
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  methods: {
    removeProject(id) {
      const index = this.projects.findIndex((pj) => pj.id == id);
      this.projects.splice(index, 1);
      localStorage.setItem("projects", JSON.stringify(this.projects));
      window.toaster.success("Project Deleted Success!", {
        position: "top-right",
        duration: 3000,
      });
    },
    deleteProject(id) {
      this.projects = this.projects.filter((project) => {
        return project.id != id;
      });
    },
    completeProject(id) {
      let findProject = this.projects.find((project) => {
        return project.id === id;
      });

      findProject.complete = !findProject.complete;
    },
  },
  components: {
    FilterNav,
    SingleProject,
  },
  computed: {
    filteredProjects() {
      if (localStorage.getItem("projects")) {
        if (this.current === "today") {
          return this.projects.filter((project) => {
            return (
              moment(project.date).format("YYYY:MM:DD") ==
              moment(new Date()).format("YYYY:MM:DD")
            );
          });
        }
        if (this.current === "complete") {
          return this.projects.filter((project) => {
            return project.complete;
          });
        }
        if (this.current === "ongoing") {
          return this.projects.filter((project) => {
            return !project.complete;
          });
        }
        return this.projects;
      }
    },
  },
  mounted() {
    let datas = localStorage.getItem("projects");
    this.projects = JSON.parse(datas);
  },
};
</script>
