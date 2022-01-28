<template>
  <div id="app">
    <AddTask @add-new-task="addNewTask" />
    <Tasks :tasks="openTasks" v-on:delete-task="deleteTask" v-on:update-task="updateTask" />
    <input type="checkbox" v-model="showCompletedTasks" id="show-completed-tasks" />
    <label for="show-completed-tasks" class="pl-2">Show Completed Tasks</label>
  </div>
</template>

<script>
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

import Axios from "axios";

export default {
  name: 'App',
  components: {
    Tasks,
    AddTask
  },

  data(){
    return {
      tasks: [],
      showCompletedTasks: true,
    }
  },
  
  methods: {
    deleteTask(id){
      // this.tasks = this.tasks.filter(task => task.id !== id);
      // this.updateStorage(this.tasks);
      // fetch('https://jsonplaceholder.typicode.com/todos/'+id,{method: 'DELETE'})
      // .then(response => response.json())
      // .then(data => console.log(data))
      // .catch(e => console.log(e));

      Axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
    .then(response => {
      if(response.status === 200) {
        this.tasks = this.tasks.filter(task => task.id != id);
      }
    })
    .catch(e => console.log(e));

    },

    addNewTask(task){
      this.tasks = [...this.tasks, task];

      Axios.post('https://jsonplaceholder.typicode.com/todos/',{
        data: { title: task.title }
      })
      .then(response => {
        if(response.status === 201){
          console.log("Task was added succesfully");
        }
      })
      .catch(e => console.log(e));
    
      this.updateStorage(this.tasks);
    },

    updateTask(id){
      // console.log(id);
      const t = "This was updated!";
      Axios.put(`https://jsonplaceholder.typicode.com/todos/${id}`, {
        data: { title: t}
      })
    .then(response => {
      if(response.status === 200){
        this.tasks = this.tasks.map(task => {
          if(task.id == id){
            task.title = t;
          }
        })
      }
    })
    .catch(e => console.log(e));
    },
    
    updateStorage(tasks){
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }
  },

  computed: {
    openTasks(){
      return this.showCompletedTasks ? this.tasks : this.tasks.filter(task => task.completed == false);
    }
  },

  mounted() {
    // const ls_tasks = localStorage.getItem('tasks');
    
    // if(ls_tasks !== null){
    //   this.tasks = JSON.parse(ls_tasks);
    // }
    
    // fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
    // .then(response => response.json())
    // .then(data => this.tasks = data)
    // .catch(e => console.log(e));

    Axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
    .then(response => this.tasks = response.data)
    .catch(e => console.log(e));
  }
}
</script>

<style>
</style>
