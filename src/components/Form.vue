<template>
  <div id="formGroup">
    <form v-on:submit.prevent="addTask" type="submit" id="formSubmit">
      <label for="taskInput">Task</label>
      <input
        :value="taskInput"
        @input="taskInput = $event.target.value"
        type="text"
        id="taskInput"
        placeholder="Enter Task"
      />
      <button id="AddTask">{{ buttonText }}</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Form',
  props: {
    tasks: Array,
    updateTask: Function,
  },
  created() {
    this.$bus.$on('edit-task', ({ task, id }) => {
      this.taskInput = task;
      this.taskId = id;
      this.buttonText = 'Save Changes';
    });
  },
  data() {
    return {
      taskInput: '',
      buttonText: 'Add Task',
      taskId: '',
    };
  },
  methods: {
    addTask() {
      this.$emit('addTask', {
        taskInput: this.taskInput,
        buttonText: this.buttonText,
      });

      //edit task
      if (this.buttonText !== 'Add Task') {
        this.tasks.forEach((task) => {
          if (task.id === this.taskId) {
            task.taskName = this.taskInput;
            task.completed = false;
            this.updateTask(task.id, this.taskInput);
          }
        });
        this.buttonText = 'Add Task';
      }
      this.taskInput = '';
    },
  },
};
</script>

<style scoped>
#formGroup {
  margin: 2rem 0;
  padding: 1rem;
  background-color: #fff;
  border-radius: 8px;
}

#formSubmit {
  display: flex;
  flex-direction: column;
}

#formSubmit input {
  margin: 0.3rem 0 1rem 0;
  padding: 0.5rem;
  outline: none;
  background-color: #dcf7f0;
  color: #53686a;
  border: 2px solid #adf9d5;
  border-radius: 8px;
}

#formSubmit button {
  padding: 0.5rem 0;
  background-color: #232dff;
  color: #fff;
  border: none;
  border-radius: 8px;
  font-size: 17px;
  cursor: pointer;
}

#formSubmit button:hover {
  background-color: #0769fe;
}
</style>
