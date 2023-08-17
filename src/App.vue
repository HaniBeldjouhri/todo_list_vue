<template>
    <div class="container">
      <div class="form">
        <input v-model="newTask" class="input" />
        <button @click="addTask" class="add">Add Task</button>
      </div>
      <Tasks :tasks="tasks" @deleteTask="deleteTask" />
    </div>
  </template>
  
  <script>
  import Tasks from "./components/Tasks.vue";
  
  export default {
    name: "App",
    components: {
      Tasks,
    },
    data() {
      return {
        newTask: "",
        tasks: [],
      };
    },
    created() {
      this.TasksFromLocal();
    },
    methods: {
      addTask() {
        if (!this.newTask) {
          return;
        }
        const counter = Date.now();
        const task = {
          id: counter,
          text: this.newTask,
        };
        this.tasks.push(task);
        window.localStorage.setItem(`task ${counter}`, this.newTask);
        this.newTask = "";
      },
      TasksFromLocal() {
        for (let i = 0; i < localStorage.length; i++) {
          const key = localStorage.key(i);
          if (key.startsWith("task")) {
            const taskId = key.replace("task", "").trim();
            const taskText = localStorage.getItem(key);
            this.tasks.push({ id: taskId, text: taskText });
          }
        }
      },
      deleteTask(taskId, updatedTasks) {
        this.tasks = updatedTasks;
        window.localStorage.removeItem(`task ${taskId}`);
      },
    },
  };
  </script>
  
  <style>
   *{
            font-family: Verdana, Geneva, Tahoma, sans-serif;
            font-size: large;
        }
        .container{
            justify-content: center;
            align-items:center;
            display:grid;
        }
        .form{
            padding: 10px;
            height: 30px;
            background-color: rgb(212, 212, 212);
            border-radius: 10px;
        }
        .input{
            width: 320px;
            height: 30px;
            border: 0;
            border-radius: 10px;
            outline:0;
        }
        .add{
            width: 100px;
            height: 30px;
            border: 0;
            border-radius: 10px;
            cursor: pointer;
        }
        .add:hover{
            color: white;
            background-color: rgb(156, 154, 154);
        }
        .tasks{
            display: block;
        }
        .task{
            padding: 10px;
            height: 30px;
            background-color: rgb(212, 212, 212);
            border-radius: 10px;
            margin: 10px;
            text-align: center;
        }
        .Delete{
            width: 100px;
            height: 30px;
            border: 0;
            border-radius: 10px;
            cursor: pointer;
            margin-left: 20px;
        }
  </style>
  
  
  
