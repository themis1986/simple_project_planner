<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div class="" v-if="projects.length">
      <div class="" v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete($event)"
          @complete="handleComplete($event)"
        />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from "@/components/SingleProject.vue";
import FilterNav from "@/components/FilterNav.vue";

export default {
  name: "Home",
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  computed: {
    filteredProjects() {
      if (this.current === "completed")
        return this.projects.filter((project) => project.complete === true);
      if (this.current === "ongoing")
        return this.projects.filter((project) => project.complete === false);
      return this.projects;
    },
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    handleComplete(id) {
      let p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err));
  },
};
</script>
