<script >
import axios from 'axios';

export default{
    data() {
      return {
        //array vuoto da pienare con la chiamata axios
        tasks: [],

        //struttura oggetto per creare nuove stask
        newTask: {
          title: "",
          status: false,
        }
      }
    },

    methods: {
      insertTask(){
        const url = 'http://localhost:8888/Boolean/php-todo-list-json/backend/newTasks.php';
        const data = this.newTask;
        const headers = {
        headers: { 'Content-Type': 'multipart/form-data' } 
      };

      if(this.newTask.title !== ""){
        axios.post(url, data, headers)
          .then(response => {this.tasks = response.data;
          this.newTask.title = ""
        })
          .catch(error => console.error("error", error));
      }
      },

      deleteTask(index){
        // console.log("task deleted", index);
        const url = 'http://localhost:8888/Boolean/php-todo-list-json/backend/deleteTasks.php';
        const data = {"index": index};
        const headers = {
        headers: { 'Content-Type': 'multipart/form-data' } 
      };

      axios.post(url, data, headers)
          .then(response => {this.tasks = response.data;
          // this.newTask.title = ""
        })
          .catch(error => console.error("error", error));
      },

      taskToggle(index){
      this.tasks[index].status = !this.tasks[index].status;
      }
    },

    //Chiamata axios 
    mounted() {
    
    axios.get('http://localhost:8888/Boolean/php-todo-list-json/backend/index.php')
         .then(response => {
            this.tasks = response.data;
         });
  }
  }


  
  
</script>

<template>
<div class="container">
  <div class="row">
    <div class="col text-center my-5">
      <h1>
        Todo List
      </h1>
    </div>
  </div>
  <div class="row">
    <div class="col-md-6 offset-md-3">
      <ul class="list-group list-group-flush mb-5">
        <li v-for="(task, index) in tasks" :key="index" class="list-group-item d-flex justify-content-between">
          <span @click="taskToggle(index)" :class="task.status ? '' : 'text-decoration-line-through'">{{task.title}}</span>
          <i @click="deleteTask(index)" class="bi bi-x-square-fill"></i>
        </li>
      </ul>
      <form @submit.prevent="insertTask" action="">
        <div class="input-group">
          <input type="text" class="form-control" placeholder="Recipient's username" aria-label="Recipient's username" aria-describedby="button-addon2" v-model="newTask.title">
          <button @click.prevent="insertTask" class="btn btn-outline-secondary" type="button" id="button-addon2">Button</button>
        </div>
      </form>
    </div>
  </div>
</div>
  
</template>

<style scoped>
</style>
