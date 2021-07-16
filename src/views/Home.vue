<template>
  <div class="home">
    <!-- we need event listener only for value in it from emit -->
    <FilterNav @filterChange="current =$event" :current="current"/>
    <div v-if="projects.length">
    <div v-for="project in projects" :key="project.id">

<!-- adding custom event for update in case of deletion  -->
<SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
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
      projects:[],
      current: 'all'
    }
  },

  mounted(){
    fetch('http://localhost:3000/projects')
    .then(res=>res.json())
    .then(data =>this.projects = data )
    .catch(err =>console.log(err.message))
  },
  methods:{
    // creating method for deletion project.id  from an array projects 
    handleDelete(id){
      this.projects = this.projects.filter((project) =>{
        // if false (project.id == id) - then item will be deleted from an array
        return project.id !== id;
      })

    },
    handleComplete(id){
      let p =this.projects.find(project =>{
        return project.id == id
      })
      p.complete = !p.complete
    }
  }

}
</script>
