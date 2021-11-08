<template>
  <div class="home">

    <FilterNav @filterChange=" current = $event" :current='current'/>
    <div v-if="projects.length">

      <div v-for="project in filterdProjects" :key="project.id">
      <SingleProject @complete="handleComplete" :project='project' @delete="handleDelete"/>

      </div>
    </div>

  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: {SingleProject, FilterNav},
  data(){
    return{
      projects: [],
      current: 'all'
    }
  },

  // This is how we collect the data from JSON server and append it to 'projects' array.
  mounted (){
    fetch('http://localhost:3000/projects')
     .then(res => res.json())
     .then(data => this.projects = data )
     .catch(err => console.log(err.message))
  },


  methods: {
    
    handleDelete(id){
      this.projects = this.projects.filter( (project) =>{
        return project.id !== id
      })
    }, 
    
    handleComplete(id){
      let p = this.projects.find( project => {
        return project.id === id
      })
      p.complete = !p.complete
    }
  },

  computed: {

    filterdProjects(){ 
      if(this.current === 'completed'){
        return this.projects.filter( project => project.complete)
      }
      if(this.current === 'ongoing'){
        return this.projects.filter( project => !project.complete)
      }
      return this.projects
    }
  }
}
</script>
