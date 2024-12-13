<template>
  <div class="card">
    <div class="task-manager">
      <h1>Task Management App</h1>

      <button class="toggle-form-btn" @click="toggleForm">
        {{ showForm ? 'HIDE ADD TASK FORM' : 'SHOW ADD TASK FORM' }}
      </button>

      <div v-if="showForm" class="form-section">
        <input v-model="newTask" placeholder="Enter task name" />
        <button class="add-task-btn" @click="addTask">Add Task</button>
      </div>

      <p v-if="tasks.length === 0" class="no-tasks">No tasks available</p>

      <ul class="task-list">
        <li
          v-for="(task, index) in tasks"
          :key="index"
          :class="{ completed: task.completed }"
        >
          <span>{{ task.name }}</span>
          <div class="task-buttons">
            <button @click="markCompleted(index)">
              {{ task.completed ? 'Undo' : 'Complete' }}
            </button>
            <button @click="deleteTask(index)">Delete</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from 'vue';

export default {
  name: 'TaskList',
  setup() {
    const state = reactive({
      tasks: JSON.parse(localStorage.getItem('tasks')) || [],
      newTask: '',
      showForm: false,
    });

    const toggleForm = () => {
      state.showForm = !state.showForm;
    };

    const addTask = () => {
      if (state.newTask.length >= 3) {
        state.tasks.push({ name: state.newTask, completed: false });
        state.newTask = '';
        saveTasks();
      } else {
        alert('Task name must be at least 3 characters long');
      }
    };

    const markCompleted = (index) => {
      state.tasks[index].completed = !state.tasks[index].completed;
      saveTasks();
    };

    const deleteTask = (index) => {
      state.tasks.splice(index, 1);
      saveTasks();
    };

    const saveTasks = () => {
      localStorage.setItem('tasks', JSON.stringify(state.tasks));
    };

    return {
      ...toRefs(state),
      toggleForm,
      addTask,
      markCompleted,
      deleteTask,
    };
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f4f4f4;
  margin: 0;
}

.card {
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 400px;
  padding: 20px;
  text-align: center;
}

.task-manager {
  font-family: Arial, sans-serif;
}

.toggle-form-btn {
  background-color: #17aef4;
  color: white;
  border: none;
  padding: 10px 15px;
  margin-bottom: 20px;
  cursor: pointer;
  font-weight: bold;
  border-radius: 5px;
}

.form-section input {
  width: 80%;
  padding: 8px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.add-task-btn {
  background-color: #400bff;
  color: white;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
  border-radius: 5px;
}

.no-tasks {
  font-size: 18px;
  color: #999;
  margin: 20px 0;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 15px;
  margin: 10px 0;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.task-list li.completed {
  background-color: rgb(144, 219, 238);
  border: 1px solid red;
}

.task-list li .task-buttons {
  display: flex;
  gap: 10px;
}

.task-list li button {
  background-color: #6b4eeb;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
}
</style>


  