<template>
  <div id="app">
    <header>
      <!-- <nav class="navbar"> -->
        <div class="logo">
          <img src="@/assets/do-to-logo.png" alt="My Logo" />
          <!-- <img src=
"https://media.geeksforgeeks.org/gfg-gg-logo.svg" 
alt="GeeksforGeeks logo" -->
        </div>
      <!-- </nav> -->
    </header>
    <div class="container">
      <h1 class="title">TODO LIST</h1>
      <hr>
      <div class="input-group">
        <input type="text"
        class="form-control" 
        placeholder="Add item..." 
        v-model="userInput" @keyup.enter="addItem">
        <button class="btn btn-success"
        @click="addItem">ADD</button>
      </div> 
      <div class="todo-table">
        <div class="table-header">
          <div class="table-cell">Task</div>
          <div class="table-cell">Actions</div>
        </div>
        <div class="table-body">
          <div class="table-row" v-for="(item, index) in filteredList" 
          :key="index" :class="{ 'even-row':
          index % 2 === 0, 'completed': item.completed }">
            <div class="table-cell" :class="{ 'completed-cell':
            item.completed }">{{ item.value }}</div>
            <div class="table-cell">
              <button class="btn btn-primary"
              @click="toggleCompleted(index)">
                {{ item.completed ? 'Undo' : 'Complete' }}
              </button>
              <button class="btn btn-info" 
              @click="editItem(index)">Edit</button>
              <button class="btn btn-danger"
              @click="deleteItem(index)">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      userInput: '',
      searchInput: '',
      list: []
    };
  },
  computed: {
    filteredList() {
      return this.list.filter(item => item.value.toLowerCase()
      .includes(this.searchInput.toLowerCase()));
    }
  },
  methods: {
    addItem() {
      if (this.userInput.trim() !== '') {
        const newItem = {
          id: Math.random(),
          value: this.userInput.trim(),
          completed: false
        };
        this.list.push(newItem);
        this.userInput = '';
      }
    },
    deleteItem(index) {
      this.list.splice(index, 1);
    },
    editItem(index) {
      const editedTodo = prompt('Edit the todo:');
      if (editedTodo !== null && editedTodo.trim() !== '') {
        this.list[index].value = editedTodo.trim();
      }
    },
    toggleCompleted(index) {
      this.list[index].completed = !this.list[index].completed;
    }
  }
};
</script>

<style>
header {
  background-color: white;
  color: white;
  padding: 10px 0;
  display: flex;
  justify-content: center;
}

/* .navbar {
  display: flex;
  justify-content: center;
} */

.logo img {
  height: 140px;
  width: auto;
}

/* Todo list styles */
#app {
  font-family: Arial, sans-serif;
}

.container {
  margin: 20px auto;
  max-width: 600px;
}

.title {
  text-align: center;
  font-size: 36px;
  font-weight: bold;
}

hr {
  border: 1px solid #ccc;
}

.input-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.form-control {
  flex: 1;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
  margin-right: 10px;
}

.btn-success {
  border-radius: 5px;
  padding: 10px 20px;
  background-color: #128f8b;
  color: white;
  border: none;
  cursor: pointer;
}

.todo-table {
  border: 1px solid #ccc;
  border-radius: 5px;
  overflow: hidden;
}

.table-header {
  display: flex;
  background-color: #f0f0f0;
  padding: 10px;
}

.table-cell {
  flex: 1;
  padding: 10px;
  font-size: 18px;
}

.table-body {
  display: grid;
}

.table-row {
  display: flex;
  border-top: 1px solid #ccc;
}

.even-row {
  background-color: #f9f9f9;
}

.completed {
  text-decoration: line-through;
}

.completed-cell {
  color: #888;
}

.btn {
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.btn-primary {
  background-color: #f8722d;
  color: white;
  border: none;
}

.btn-info {
  background-color: #128f8b;
  color: white;
  border: none;
}

.btn-danger {
  background-color: #dc3545;
  color: white;
  border: none;
}
button{
margin-left:3px;
}
</style>