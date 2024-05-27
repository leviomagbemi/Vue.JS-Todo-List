<template>
  <div
    :key="index"
    v-for="(task, index) in tasks"
    class="taskContainer"
    :id="task.id"
  >
    <div class="task">
      <p>{{ task.taskName }}</p>
      <span>
        <i
          :class="task.completed ? `fas fa-check-double` : `far fa-square`"
          @click="changeState(task.id)"
          :id="task.id"
        ></i>
      </span>
    </div>
    <div class="editDelete">
      <span>
        <i @click="deleteTask(task.id)" class="fas fa-trash" id="delete"></i>
      </span>
      <span>
        <i
          @click="editTask(task.taskName, task.id)"
          class="fas fa-edit"
          id="edit"
        ></i>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Task',
  data() {
    return {
      completed: '',
    };
  },
  props: {
    tasks: Array,
    deleteTask: Function,
    updateTaskStatus: Function,
  },
  methods: {
    changeState(id) {
      this.tasks.forEach((task) => {
        if (task.id === id) {
          task.completed = !task.completed;
          this.updateTaskStatus(id);
        }
      });
    },
    editTask(task, id) {
      this.$bus.$emit('edit-task', { task, id });
    },
  },
};
</script>

<style scoped>
.taskContainer {
  padding: 1rem;
  background-color: #fff;
  border-radius: 24px;
  margin: 1rem 0;
}

.task {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
}

.task p {
  color: #162f40;
}

.editDelete {
  display: flex;
  gap: 1rem;
}

.editDelete span {
  cursor: pointer;
  color: #232dff;
}
</style>
