<template>
  <div class="todo-list">
    <h1>To-Do List</h1>

    <!-- Add a new task -->
    <div class="add-task">
      <input type="text" v-model="newTaskText" @keyup.enter="addTask" placeholder="Add a new task" />
      <button @click="addTask">Add</button>
    </div>

    <!-- Display tasks -->
    <h2>List Rendering Examples:</h2>

    <div>
      <h3>b. v-model with <input type="checkbox">:</h3>
      <ul>
        <li v-for="task in tasks" :key="task.id">
          <input type="checkbox" v-model="task.completed" />
          {{ task.text }}
        </li>
      </ul>
    </div>

    <div>
      <h3>c. v-for on &lt;div&gt;:</h3>
      <ul>
        <li v-for="task in completedTasks" :key="task.id">
          {{ task.text }} (Completed)
        </li>
        <li v-for="task in incompleteTasks" :key="task.id">
          {{ task.text }} (Not Completed)
        </li>
      </ul>
    </div>

    <div>
      <h3>d. v-for with v-if:</h3>
      <ul>
        <li v-for="task in completedTasks" :key="task.id">
          {{ task.text }}
        </li>
      </ul>
    </div>

    <div>
      <h3>e. v-for with a Component and Slots:</h3>
      <ul>
        <TaskItem
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @remove-task="removeTask(task)"
        >
          <template #default>
            <span>{{ task.text }}</span>
          </template>
        </TaskItem>
      </ul>
    </div>

    <div>
      <p>Total Tasks: {{ totalTasks }}</p>
    </div>

    <!-- Custom Content Slot -->
    <div class="custom-content">
      <slot name="custom-content"></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTaskText: "",
      tasks: [
        { id: 1, text: "Task 1", completed: false },
        { id: 2, text: "Task 2", completed: true },
        { id: 3, text: "Task 3", completed: false },
      ],
    };
  },
  computed: {
    completedTasks() {
      return this.tasks.filter((task) => task.completed);
    },
    incompleteTasks() {
      return this.tasks.filter((task) => !task.completed);
    },
    totalTasks() {
      return this.tasks.length;
    },
  },
  methods: {
    addTask() {
      if (this.newTaskText.trim() === "") return;

      this.tasks.push({
        id: this.tasks.length + 1,
        text: this.newTaskText,
        completed: false,
      });

      this.newTaskText = "";
    },
    // Define other methods here
  },
};
</script>

<style scoped>
/* Add your CSS styles for the to-do list here */
.todo-list {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
}

.add-task {
  display: flex;
  margin-top: 20px;
}

.add-task input {
  flex-grow: 1;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.add-task button {
  padding: 5px 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin: 10px 0;
  display: flex;
  align-items: center;
}

input[type="checkbox"] {
  margin-right: 10px;
}

button {
  padding: 5px 10px;
  background-color: #DC3545;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.custom-content {
  margin-top: 20px;
  border: 1px solid #ccc;
  padding: 10px;
}
</style>