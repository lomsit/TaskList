<template>
    <div>
      <h1>Список задач</h1>
      <TaskList :tasks="tasks" @update-task="updateTaskStatus" />
    </div>
  </template>
  
  <script>
  import TaskList from "../components/TaskList.vue";
  
  export default {
    name: "Home",
    components: {
      TaskList
    },
    data() {
      return {
        tasks: []
      };
    },
    mounted() {
      this.loadTasks();
    },
    methods: {
      loadTasks() {
        // Попробуем загрузить данные из localStorage
        const savedTasks = localStorage.getItem("tasks");
        if (savedTasks) {
          this.tasks = JSON.parse(savedTasks);
        } else {
          // Если нет данных в localStorage, загружаем из tasks.json
          fetch("/tasks.json")
            .then(response => response.json())
            .then(data => {
              this.tasks = data;
              this.saveTasks();
            })
            .catch(err => {
              console.error("Ошибка загрузки данных:", err);
            });
        }
      },
      updateTaskStatus(updatedTask) {
        const taskIndex = this.tasks.findIndex(task => task.id === updatedTask.id);
        if (taskIndex !== -1) {
          this.tasks[taskIndex] = updatedTask;
          this.saveTasks();
        }
      },
      saveTasks() {
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      }
    }
  };
  </script>