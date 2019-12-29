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
        <li v-for="(todo, key) in todos" :key="key">          
          <div class="label"
              v-bind:class="{ done: !(todo.value < todo.number)}">
            {{todo.label}}
          </div>
          <div class="value"
              v-bind:class="{ done: !(todo.value < todo.number)}">
            {{todo.value}}
          </div>
          <div class="numb"
              v-bind:class="{ done: !(todo.value < todo.number)}">
            {{todo.number}}
          </div>
          <div class="prob"
              v-bind:class="{ done: !(todo.value < todo.number)}">
            {{Math.round(todo.value*100/todo.number * 10) /10 }}%
          </div>
          <button v-bind:class="{ disable: !(todo.value < todo.number)}"
              @click="todo.value++;saveTodo()">
            ＋
          </button>
          <button v-bind:class="{ disable: todo.value == 0}"
              @click="todo.value--;saveTodo()">
            ー
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
section {
  margin: 0 auto;
  max-width: 400px;
}
h1 {
  text-align: center;
}
button {
  background-color: #fff;
  border: 1px solid #888;
}
button:hover {
  background-color: #eee;
}
input {
  background-color: #fff;
  border: 1px solid #888;
}
.todoFormBox {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
     -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
     -ms-flex-direction: row;
         flex-direction: row;
  -ms-flex-wrap: wrap;
      flex-wrap: wrap;
}
.todoFormBox input,
.todoFormBox button {
  box-sizing: border-box;
  font-size: 15px;
  height: 30px;
  padding: 5px;
}
.todoFormBox input[type="text"] {
   -webkit-box-flex: 1;
  -ms-flex-positive: 1;
          flex-grow: 1;
}
.todoFormBox input[type="number"] {
  -ms-flex-preferred-size: 60px;
               flex-basis: 60px;
  width: 60px;
}
.todoFormBox button {
  -ms-flex-preferred-size: 60px;
               flex-basis: 60px;
  width: 60px;
}
.todoListBox {
  width: 100%;
}
.todoListBox ul {
  list-style: none;
  padding: 0;
}
.todoListBox li {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
     -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
     -ms-flex-direction: row;
         flex-direction: row;
  -ms-flex-wrap: wrap;
      flex-wrap: wrap;
  padding: 5px 0;
}
.todoListBox div,
.todoListBox button {
  box-sizing: border-box;
  height: 40px;
}
.todoListBox div {
  font-size: 15px;
  line-height: 40px;
}
.todoListBox div.label {
   -webkit-box-flex: 1;
  -ms-flex-positive: 1;
          flex-grow: 1;
}
.todoListBox div.value,
.todoListBox div.numb {
  -ms-flex-preferred-size: 30px;
               flex-basis: 30px;
  width: 30px;
}
.todoListBox div.value {
  padding-right: 5px;
  text-align: right;
}
.todoListBox div.numb::before {
  content: "/";
}
.todoListBox div.prob {
  -ms-flex-preferred-size: 50px;
               flex-basis: 50px;
  padding-right: 5px;
  text-align: right;
  width: 50px;
}
.todoListBox button {
  -ms-flex-preferred-size: 40px;
               flex-basis: 40px;
  font-size: 15px;
  width: 40px;
  text-decoration: none!important;
}
.configBox {
  text-align: center;
}
.configBox button {
  font-size: 15px;
  height: 30px;
  text-transform: uppercase;
}
.done { color: #ccc; }
.disable {  pointer-events: none;}
</style>
