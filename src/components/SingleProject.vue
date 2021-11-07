<template>

  <div class="project" :class="{complete : project.complete}">

      <div class="actions" >
          <h3  @click="showDetails = !showDetails"> {{ project.title }} </h3>
          <div class="icons">

             <router-link :to="{ name: 'EditProject', params: { id: project.id }}">
              <span class="material-icons">edit</span>
             </router-link> 

              <span class="material-icons" @click="deleteProject">delete</span>
              <span class="material-icons tick" @click="toggleComplete">done</span>

          </div>
      </div>

      <div v-if="showDetails">
          {{project.details}}
      </div>
  </div>
</template>

<script>
export default {
    props:['project'],

    data(){
        return{
            showDetails: false,
            uri: 'http://localhost:3000/projects/' + this.project.id,
        }
    },

    methods:{

        deleteProject(){
            fetch(this.uri, {method: 'DELETE'})
             .then(()=> this.$emit('delete', this.project.id)) //sync json with local stored data
             .catch((err)=> console.log(err))
        },
        
        toggleComplete(){
            fetch(this.uri, {  //fetch req to db
                method: 'PATCH',
                headers: { 'Content-Type': 'application/json'},
                body: JSON.stringify({ complete : !this.project.complete})
            
            }).then( ()=> { // for syncing local data
                this.$emit('complete', this.project.id)
            }).catch( err => console.log(err))
        }
    },
}
</script>

<style>
.project{
    background: white;
    margin: 20px auto;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 4px solid #e90074;
}
h3{
    cursor: pointer;
}
.actions{
    display: flex;
    justify-content: space-between;
    align-items: center;
    
}
.material-icons{
    font-size: 22px;
    margin-left: 7px;
    color: #bbb;
    cursor: pointer;
}
.material-icons:hover{
    color: #777;
}
/* .project .tick{
    color: #e90074;
} */
.project.complete{
    border-left: 4px solid rgb(15, 196, 126);
}
.project.complete .tick{
    color: rgb(15, 196, 126);
}

</style>