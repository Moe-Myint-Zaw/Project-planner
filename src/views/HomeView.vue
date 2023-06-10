<template>
  <div class="home">
      <h1>Home</h1>
      <FilterNav @filterValue="current=$event" :current="current"></FilterNav>
      <div v-for="project in filterProject" :key="project.id">
        <single-project :project='project' @delete="deleteProject" @complete="completeProject"></single-project>
      </div>
  </div>
</template>

<script>
import SingleProject from '@/components/SingleProject.vue';
import FilterNav from '@/components/FilterNav.vue';




export default {
  name: 'HomeView',
  components: {
    SingleProject,
    FilterNav
    
  },
  data(){
    return {
      projects:[],
      showDetail:false,
      current:'all'
    }
  },
  computed:{
      filterProject(){
        if(this.current==='complete'){
          return this.projects.filter((project)=>{
                return project.complete;
          })
        }
        if(this.current==='ongoing'){
          return this.projects.filter((project)=>{
                return !project.complete;
          })
        }
         return this.projects
      }
  },
  methods:{
    deleteProject(id){
      this.projects=this.projects.filter(project=>{
        return project.id!=id;
      })
    },
    completeProject(id){
      let findProject = this.projects.find(project=>{
        return project.id===id;
      })
      findProject.complete=!findProject.complete;
    }
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then((response)=>{
        return response.json();
    })
    .then((projects)=>{
        this.projects = projects;
    })
    .catch((err)=>{
        console.log (err.Message());
    })
  }
}
</script>

<style scoped>

</style>
