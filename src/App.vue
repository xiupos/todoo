<template>
  <section>
    <div class="todoFormBox">
      <input type="text"
        v-model="newTodoLabel">
      <input type="number"
        v-model="newTodoNumber">
      <button v-on:click="makeTodo">TODOO</button>
    </div>
    <div class="todoListBox">
      <ul>
        <li v-for="(todo, key) in todos" :key="key"
            v-bind:class="{ done: !(todo.value < todo.number)}">
          "{{todo.label}}" 
          {{todo.value}} / {{todo.number}},
          {{todo.value*100/todo.number}}%
          <button v-if="todo.value < todo.number"
              @click="todo.value++">
            PLUS
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
  data: function () {
    return  {
      todos: [],
      newTodoLabel: ""
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
    },
    deleteDoneTodo: function(){
      this.todos = this.todos.filter(function (todo) {
        return todo.value < todo.number;
      });
    },
  }
}
</script>

<style>
.done { text-decoration: line-through; }
</style>
