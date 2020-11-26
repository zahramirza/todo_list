<template>
  <div class="wrapper">
    <h1>What should I do?</h1>
    <div class="newtodo">
      <input
        type="text"
        name="todo-text"
        v-model.trim="newTodoText"
        class="todo-text"
        placeholder="New todo"
      />
      <button class="todo-add-button" v-on:click="addTodo()">Add</button>
    </div>
    
    <ul v-if="todos.length">
      <li class="todo">
        <span class="todo-text list-header">Todo</span>
        <span class="todo-empty-button list-header"></span>
      </li>

      <li class="todo" v-for="todo in todos" :key="todo.id">
        <span class="todo-text">{{ todo.text }}</span>
        <button class="todo-remove-button" v-on:click="removeTodo(todo)">Remove</button>
      </li>
    </ul>
    <p class="none" v-else>Add a new todo in the input above</p>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {
  },
  data() {
    return {
      newTodoText: "",
      todos: [],
    };
  },
  methods: {
    addTodo() {
      if (this.newTodoText) {

        this.todos.push({
          text: this.newTodoText,
          id: Date.now(),
          done: false
        });

        this.newTodoText = "";
      }
    },
    removeTodo (item) {
      this.todos = this.todos.filter((_item) => _item !== item);
    }
  },
};
</script>

<style>
html,
body {
  font: 16px/1.2 BlinkMacSystemFont, -apple-system, "Segoe UI", Roboto,
    Helvetica, Arial, sans-serif;
  padding: 10px;
}

.wrapper {
  width: 75%;
  margin: 0 auto;
}

.newtodo {
  display: flex;
  width: 100%;
}

.todo {
  display: flex;
  width: 100%;
  margin-bottom: 5px;
}

.list-header {
  text-align: center;
  padding: 5px;
  margin-left: 2px;
  margin-right: 2px;
  background-color: #eee;
  color: black;
}

.todo-text {
  flex: 4;
  text-align: center;
}

.todo-date {
  flex: 3;
  text-align: center;
}

.todo-add-button {
  flex: 1;
  border: 1px solid green;
  background: green;
  color: white;
  font-size: 0.8rem;
  padding: 2px 4px;
  cursor: pointer;
  margin-left: 2px;
  margin-right: 2px;
  outline: none;
}

.todo-remove-button {
  flex: 1;
  border: 1px solid red;
  background-color: red;
  color: white;
  font-size: 0.8rem;
  padding: 2px 4px;
  cursor: pointer;
  margin-left: 2px;
  margin-right: 2px;
}

.todo-empty-button {
  flex: 1;
  padding: 2px 4px;
  margin-left: 2px;
  margin-right: 2px;
}

ul, li {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

ul {
    margin-top: 40px;
}

button {
  border: 1px solid green;
  background: green;
  color: white;
  font-size: 0.8rem;
  padding: 2px 4px;
  cursor: pointer;
}

p.none {
  color: #888;
  font-size: small;
}
</style>
