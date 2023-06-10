<template>
<div class="flex">

<div class="project" :class="{complete:project.complete}">
    <h3 @click="showDetail=!showDetail">{{project.title}}</h3>
    <p v-if="showDetail">{{project.detail}}</p>
</div>
   
<div>
    <span class="material-icons" @click="deleteProject">
            delete
    </span>
    <router-link :to="{name:'EditProject',params:{id:project.id}}">
        <span class="material-icons">
            edit
        </span>
    </router-link>
   
    <span class="material-icons" @click="completeProject">
            done
    </span>
</div>
    




</div>   
</template>

<script>
export default {
    props:['project'],
    data(){
        return{
            showDetail:false,
            api:'http://localhost:3000/projects/'
        }
    },
    methods:{
        deleteProject(){
            let deleteRoute=this.api+this.project.id;
            fetch(deleteRoute,{method:"DELETE"})
            .then(()=>{
                this.$emit("delete",this.project.id)
            })
            .catch((err)=>{
                console.log (err.Message());
            })
        },
        completeProject(){
            let updateCompleteRoute=this.api+this.project.id;
            fetch(updateCompleteRoute,{
                method:"PATCH",
                headers:{
                    "Content-Type":"application/json"
                },
                body:JSON.stringify(
                    {
                        complete:!this.project.complete
                    }
                )

                })
            .then(()=>{
                this.$emit("complete",this.project.id)
            })
            .catch((err)=>{
                console.log (err.Message());
            })
        }
    }


}
</script>

<style>
.project{
  padding:20px;
  background-color: #f2f2f2;
  margin: 10px;
  border-left: 6px solid crimson;
  border-radius: 8px;

}
h3{
  cursor: pointer;
  color:indigo;
}
.flex{
    display: flex;
    justify-content: space-between;
    align-items: center;
    }
span{
    margin-left: 10px;
}
span:hover{
    cursor: pointer;
    color: #777;
}
.complete{
  border-left: 6px solid green  ;
}
</style>