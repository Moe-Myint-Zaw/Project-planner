<template>
  
    <h1>Edit Project</h1>
  <form @submit.prevent="editProject">
    <label>Project Title</label>
    <input type="text" v-model="title">
    <br>
    <br>
    <label>Project detail</label>
    <input type="text" v-model="detail">
    <button>Upate Project</button>
  </form>
</template>


<script>
export default {
  props:['id'],
    data(){
        return{
            title:"",
            detail:""
        }
    },
    mounted(){
    fetch('http://localhost:3000/projects/'+this.id)
    .then((response)=>{
        return response.json();
    })
    .then((datas)=>{
        this.title = datas.title;
        this.detail=datas.detail;
    })
    .catch((err)=>{
        console.log (err.Message());
    })
  },
    methods: {
        editProject(){
            fetch('http://localhost:3000/projects/'+this.id,{
                method:"PATCH",
                headers:{
                    "Content-Type":"application/json"
                },
                body:JSON.stringify(
                    {
                        title: this.title,
                        detail: this.detail,
                    }
                )
            })
            .then(()=>{
                this.$router.push('/');
            })
            .catch((err)=>{
                console.log(err);
            })
        }

    }
}
</script>

<style>

</style>