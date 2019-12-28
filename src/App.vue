<template>
  <section>
    <h1>TODOO</h1>
    <div class="todoFormBox">
      <input type="text"
        v-model="newTodoLabel">
      <input type="number"
        v-model="newTodoNumber">
      <button v-on:click="makeTodo">LIST</button>
    </div>
    <div class="todoListBox">
      <ul>
        <li v-for="(todo, key) in todos" :key="key"
            v-bind:class="{ done: !(todo.value < todo.number)}">
          "{{todo.label}}" 
          {{todo.value}} / {{todo.number}},
          {{Math.round(todo.value*100/todo.number * 10) /10 }}%
          <button v-bind:class="{ disable: !(todo.value < todo.number)}"
              @click="todo.value++;saveTodo()">
            +
          </button>
          <button v-bind:class="{ disable: todo.value == 0}"
              @click="todo.value--;saveTodo()">
            -
          </button>
        </li>
      </ul>
    </div>
    <div class="configBox">
      <button v-on:click="deleteDoneTodo">Delete done todoos</button>
    </div>
  </section>
</template>

<script>
export default {
  mounted: function(){
    this.loadTodo();
  },
  data: function () {
    return  {
      todos: [],
      newTodoLabel:   "",
      newTodoNumber:  ""
    }
  },
  methods: {
    makeTodo: function () {
      if(this.newTodoLabel)
        this.todos.push({
          label:  this.newTodoLabel,
          value:  0,
          number: this.newTodoNumber>1?this.newTodoNumber:1,
          check:  false
        });
      this.newTodoLabel = "";
      this.newTodoNumber = "";
      this.saveTodo();
    },
    deleteDoneTodo: function(){
      this.todos = this.todos.filter(function (todo) {
        return todo.value < todo.number;
      });
      this.saveTodo();
    },
    saveTodo: function(){
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },
    loadTodo: function(){
      this.todos = JSON.parse( localStorage.getItem('todos') );
      if( !this.todos ){
        this.todos = [];
      }
    },
  }
}
</script>

<style>
.done { text-decoration: line-through; }
.disable {
  pointer-events: none;
}
</style>
