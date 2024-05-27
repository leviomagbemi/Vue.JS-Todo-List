<template>
  <div class="tasksContainer">
    <Task
      v-if="tasks.length > 0"
      :tasks="tasks"
      :delete-task="deleteTask"
      :update-task-status="updateTaskStatus"
    />
    <div v-else class="noTasks">No Tasks Available</div>
  </div>
</template>

<script>
import Task from './Task';
export default {
  name: 'Tasks',
  props: {
    tasks: Array,
    deleteTask: Function,
    updateTaskStatus: Function,
  },
  components: {
    Task,
  },
};
</script>

<style scoped>
.tasksContainer {
  margin: 2rem 0;
  padding: 1rem;
  background-color: #f3f4f7;
  border-radius: 8px;
  max-height: 200px;
  overflow: hidden scroll;
}

.tasksContainer::-webkit-scrollbar {
  width: 5px;
  height: 8px;
  background-color: #f3f4f7;
}

.tasksContainer::-webkit-scrollbar-thumb {
  background: #232dff;
  border-radius: 4px;
}

.noTasks {
  text-align: center;
  font-size: 1.5rem;
  font-weight: 500;
}
</style>
