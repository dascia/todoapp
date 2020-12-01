<template>
  <div id="todo-container">
    <div>
      <input
        type="text"
        name="task-name"
        id="task-title"
        class="task-title"
        placeholder="Write your task..."
        v-model="newTask"
        @keyup.enter="addTask"
      />
    </div>
    <ul id="tasks-container">
      <li class="task" v-for="task in tasks" :key="task.id">
        <div>
          <span class="task-title" :class="{'task-completed' : task.done}" >{{ task.content }}</span>
          <button class="btn-action action" data-task-action="complete" @click="toggleTaskState(task.id)">
            <i class="fas fa-check"></i>
          </button>
          <button class="btn-action action-delete" data-task-action="delete" @click="deleteTask(task.id)">
            <i class="fas fa-trash"></i>
          </button>
        </div>
        <div></div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
// Imports
import { ref } from 'vue';

// Interface
interface Task {
  id: number;
  content: string;
  done: boolean;
}

// Components
export default {
  name: 'TodoList',
  setup() {
    const tasks = ref<Task[]>([]);
    const newTask = ref<string>('');

    // Methods

    // Add a task to the list
    function addTask(): void {
      if (newTask.value.trim() === '') return;
      const id = new Date().valueOf();
      tasks.value.push({ id: id, content: newTask.value, done: false });
      newTask.value = '';
    }

    // Delete a task from the list
    function deleteTask(taskId: string): void {
      console.log("Taks id: ", taskId);
      const taskIndex = tasks.value.findIndex(_ => _.id == +taskId);
      if (taskIndex == -1) return;
      console.log("Task index: ", taskIndex);
      tasks.value.splice(taskIndex, 1);
    }

    // Toggle completition status of the task
    function toggleTaskState(taskId: string): void {
      const taskIndex: number = tasks.value.findIndex(_ => _.id == +taskId) as number;
      if (taskIndex == -1) return;
      tasks.value[taskIndex].done = !tasks.value[taskIndex].done;
    }

    return {
      tasks,
      newTask,
      addTask,
      deleteTask,
      toggleTaskState,
    };
  },
};
</script>

<style scoped>
#todo-container {
  width: 40%;
  min-width: 400px;
  height: 60%;
  min-width: 600px;
}

#task-title {
  width: 100%;
  height: 40px;
  padding: 0px 0px 0px 10px;
  box-sizing: border-box;
  font-family: 'Open Sans';
  outline: none;
  font-size: 1em;
  border-top: 0px;
  border-left: 0px;
  border-right: 0px;
  border-bottom: 1px solid #f0f0f0;
  box-shadow: 0px 1px 4px 1px rgba(0, 0, 0, 0.1);
}

#tasks-container {
  background-color: white;
  margin: 0;
  padding: 0;
  list-style: none;
  margin: 10px 0 0 0;
  border: 1px solid #f0f0f0;
  height: 100%;
  box-shadow: 0px 1px 4px 1px rgba(0, 0, 0, 0.1);
}

.task {
  display: flex;
  flex-direction: column;
  padding: 10px 8px 0 8px;
}

/* task box including actions */
.task > div:first-child {
  flex-direction: row;
  display: flex;
  align-items: center;
}

/* line separator */
.task > div:last-child {
  margin-top: 8px;
  border-bottom: 1px solid #e8e8e8;
}

.task-title {
  font-size: 0.9em;
  color: #4b4c4c;
  font-family: 'Roboto';
  font-weight: 400;
  flex: 1 0 auto;
}

.btn-action {
  outline: none;
  font-size: 1em;
  border: none;
  background-color: transparent;
  cursor: pointer;
}

.action-delete:hover {
  color: red;
}

.action:hover {
  color: lightseagreen;
}

.task-completed {
  text-decoration: line-through;
}
</style>
