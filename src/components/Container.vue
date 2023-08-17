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
  import Tasks from "./Tasks.vue";
  
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
        this.tasks.push({text: this.newTask});
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
