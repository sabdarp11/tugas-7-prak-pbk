<template>
  <div class="todo-app">
    <h1>Todo List</h1>
    <div class="input-container">
      <input v-model="newTask" placeholder="Add a new task" @keyup.enter="addTask" />
      <button class="add-btn" @click="addTask">Add</button>
    </div>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <input type="checkbox" :checked="task.completed" @change="toggleTaskCompletion(index)" />
        <span :class="{ completed: task.completed }">{{ task.text }}</span>
        <button class="remove-btn" @click="removeTask(index)">Remove</button>
      </li>
    </ul>
    <p>Total incomplete tasks: {{ incompleteTasksCount }}</p>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { useTodoStore } from './stores/todoStore';

export default {
  setup() {
    const todoStore = useTodoStore();
    const newTask = ref('');

    const addTask = () => {
      if (newTask.value.trim()) {
        todoStore.addTask(newTask.value);
        newTask.value = '';
      }
    };

    const toggleTaskCompletion = (index) => {
      todoStore.toggleTaskCompletion(index);
    };

    const incompleteTasksCount = computed(() => todoStore.incompleteTasksCount);

    return {
      newTask,
      tasks: todoStore.tasks,
      incompleteTasksCount,
      addTask,
      removeTask: todoStore.removeTask,
      toggleTaskCompletion
    };
  }
};
</script>

<style>
body {
  background-image: url('/src/assets/bmw1.jpg'); /* Sesuaikan path dengan lokasi gambar */
  background-size: 200vh;
  margin: 0; /* Reset margin agar tidak ada ruang putih di sekitar body */
  padding: 0; /* Reset padding agar tidak ada ruang putih di sekitar body */
  font-family: Arial, sans-serif;
}

.todo-app {
  max-width: 600px;
  margin: 20px auto; /* Mengatur margin pada todo-app */
  text-align: center;
  padding: 20px;
  border-radius: 8px;
  background-color: rgba(255, 255, 255, 0.8); /* Menambahkan transparansi pada background todo-app */
  /* background-image: none; */ /* Menonaktifkan background image pada todo-app */
}




.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

input[type="text"] {
  padding: 10px;
  width: calc(100% - 130px);
  border: 1px solid #ddd;
  border-radius: 4px;
  outline: none;
  transition: border-color 0.3s ease;
  font-size: 16px;
}

input[type="text"]:focus {
  border-color: #4caf50;
}

button {
  padding: 10px 20px;
  margin-left: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease, box-shadow 0.3s ease;
  font-size: 16px;
}

button.add-btn {
  background: linear-gradient(45deg, #4caf50, #66bb6a);
  color: white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

button.add-btn:hover {
  background: linear-gradient(45deg, #43a047, #5cb85c);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
}

button.remove-btn {
  background: linear-gradient(45deg, #f44336, #e57373);
  color: white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

button.remove-btn:hover {
  background: linear-gradient(45deg, #e53935, #d32f2f);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ccc;
}

li .completed {
  text-decoration: line-through;
  color: #888;
}

@media (max-width: 600px) {
  .todo-app {
    padding: 10px;
  }

  .input-container {
    flex-direction: column;
  }

  input[type="text"] {
    width: 100%;
    margin-bottom: 10px;
  }

  button {
    width: 100%;
    margin-left: 0;
  }

  li {
    flex-direction: column;
    align-items: flex-start;
  }

  li button {
    margin-top: 10px;
  }
}
</style>
