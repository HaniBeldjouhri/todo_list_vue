<template>
  <div class="task">
    {{ task.text }}
    <button @click="deleteTask" class="Delete">Delete</button>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    deleteTask() {
      this.$emit("deleteTask", this.task.id);
    },
  },
};
</script>
