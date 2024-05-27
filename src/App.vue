<template>
  <div id="container">
    <h1 id="logo"><i class="fas fa-clipboard-list"></i> TodoPro</h1>
    <Form @add-task="addTask" :tasks="tasks" :update-task="updateTask" />
    <Filter @filter-task="filterTask" />
    <Tasks
      :tasks="tasks"
      :delete-task="deleteTask"
      :update-task-status="updateTaskStatus"
    />
  </div>
</template>

<script>
import Form from './components/Form';
import Filter from './components/Filter';
import Tasks from './components/Tasks';
export default {
  name: 'App',
  created() {
    this.tasks = this.getFromStorage();
  },
  data() {
    return {
      tasks: [],
    };
  },
  components: {
    Form,
    Filter,
    Tasks,
  },

  methods: {
    //add task to storage
    addToStorage(task) {
      let tasks = [];

      if (localStorage.getItem('tasks') === null) {
        tasks = [];
      } else {
        tasks = JSON.parse(localStorage.getItem('tasks'));
      }

      tasks.push(task);

      localStorage.setItem('tasks', JSON.stringify(tasks));
    },

    //get task from storage
    getFromStorage() {
      let tasks;

      if (localStorage.getItem('tasks') === null) {
        tasks = [];
      } else {
        tasks = JSON.parse(localStorage.getItem('tasks'));
      }

      return tasks;
    },

    //update task from storage
    updateTaskStatus(id) {
      let tasks = JSON.parse(localStorage.getItem('tasks'));

      tasks.forEach((task) => {
        if (task.id === id) {
          task.completed = !task.completed;
          console.log(typeof id, typeof task.id);
        }

        localStorage.setItem('tasks', JSON.stringify(tasks));
      });
    },

    //update task from storage
    updateTask(id, taskInput) {
      let tasks = JSON.parse(localStorage.getItem('tasks'));

      tasks.forEach((task) => {
        if (task.id === id) {
          task.completed = false;
          task.taskName = taskInput;
        }
        this.tasks = tasks;
        localStorage.setItem('tasks', JSON.stringify(tasks));
      });
    },

    //add tasks
    addTask({ taskInput, buttonText }) {
      if (taskInput !== '' && buttonText === 'Add Task') {
        this.addToStorage({
          taskName: taskInput,
          completed: false,
          id: Math.random().toString(16).slice(2),
        });
        this.tasks = this.getFromStorage();
      } else if (taskInput === '') {
        alert('Fill all fields');
      }
    },

    filterTask(value) {
      switch (value) {
        case 'completed tasks':
          this.tasks = this.getFromStorage().filter(
            (task) => task.completed === true
          );
          console.log(this.tasks);
          break;

        case 'incomplete tasks':
          this.tasks = this.getFromStorage().filter(
            (task) => task.completed === false
          );
          break;

        case 'all tasks':
          this.tasks = this.getFromStorage();
          break;
      }
    },

    deleteTask(id) {
      let tasks = JSON.parse(localStorage.getItem('tasks'));

      tasks.forEach((task, index) => {
        if (task.id === id) {
          tasks.splice(index, 1);
        }
        this.tasks = tasks;
        localStorage.setItem('tasks', JSON.stringify(tasks));
      });
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  line-height: 1.26;
  font-family: poppins;
}

#container {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1rem;
  border: 2px solid #272727;
  border-radius: 8px;
  background-color: #ebeefa;
}

#logo {
  color: #2d2e31;
  text-align: center;
}
</style>
