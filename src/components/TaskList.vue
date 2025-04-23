<template>
    <div>
      <ul>
        <li v-for="task in tasks" :key="task.id">
          <input 
            type="checkbox" 
            v-model="task.done" 
            @change="updateTaskStatus(task)" 
          />
          {{ task.title }}
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: "TaskList",
    props: {
      tasks: Array
    },
    methods: {
      updateTaskStatus(task) {
        this.$emit("update-task", task);
      }
    }
  };
  </script>