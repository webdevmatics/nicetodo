<template>
  <div>

    <h1>{{appTitle}}</h1>

    <h3>
      List of todo 
    </h3>

      <input type="text" v-model="newTodoTitle" placeholder="enter your task" @keydown.enter="addTodo">

      <button v-if="newTodoTitle" @click="addTodo" >Add</button>
    <ul>
      <li v-for="(todo,index) in todos" :key="index">

        <input v-if="todo.editMode" type="text" v-model="todo.name" @keydown.enter="updateTodo(todo)">
        <span v-else :class="{completed: todo.done}">{{todo.name}} </span> 
        
        <div>
        <button @click="toggleDone(todo)">{{todo.done ? 'mark undone': 'mark done'}}</button>
        <button @click="removeTodo(todo)">del</button>
        <button @click="editTodo(todo)">{{todo.editMode? 'cancel edit': 'edit'}}</button>
        </div>
        

      </li>
     
    </ul>


  </div>

</template>

<script>
const STORAGE_KEY = 'nice-todo';
export default {
  data(){
    return {
      idCounter: 0,
      newTodoTitle: '',
      appTitle: "Nice Todo app",
      todos: JSON.parse(localStorage.getItem(STORAGE_KEY)) || [] 
    }
  },

  watch:{

    todos : {
      handler(newTodos) {
          localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todos));
      },

      deep: true
    }
 
  
  },

  methods:{
    addTodo(){
      this.todos.push({
        id: this.idCounter,
        name: this.newTodoTitle,
        done: false,
        editMode: false
      })
      this.newTodoTitle = ''
      this.idCounter++;

    },

    toggleDone(todo){
      todo.done = !todo.done;
    },
    
    removeTodo(todo){
      this.todos = this.todos.filter(item=> item.id != todo.id)
    },
    editTodo(todo) {

      todo.editMode = ! todo.editMode;

    }
    ,
    updateTodo(todo) {
        todo.editMode = false;
    }

  },


  created(){
    if(this.todos.length) {
      this.idCounter = this.todos[this.todos.length-1].id + 1
    }
  }


}
</script>

<style>

.completed{
  text-decoration: line-through;
}

</style>