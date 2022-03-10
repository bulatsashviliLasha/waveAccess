<template>
 <div class="container">

    <article>
      <h1>Need to buy</h1>
      <form @submit.prevent>

        <input
            v-model="newTodo"
            name="newTodo"
        >
        <button @click="modal">Add</button>
      </form>
    </article>
     <article>
       <h2>Purchased</h2>
       <ul>
         <li
             v-for="(todo, index) in todos"
             :key="index"
         >
            <span
                @click="doneTodo(todo)"
            >{{ todo.content }}</span> &nbsp;
           <button @click="removeTodo(index)">Remove</button>
         </li>
       </ul>
       <h4 v-if="todos.length === 0">Empty list.</h4>

     </article>
   <main :class="{
     'noContent' : !showOrHide
   }">
     <button @click="addTodo();modal()" >Confirm</button>
     <button @click="modal()">Decline</button>
   </main>
 </div>
</template>

<script>
import { ref , watch} from 'vue';
export default {
  data(){
    return {
      showOrHide : false,
    }
  },
  methods : {
    modal(){
      this.showOrHide = !this.showOrHide
    }
  }
  ,

  setup () {
    const newTodo = ref('');
    const defaultData = [{
      done: false,
      content: 'Write a blog post'
    }]
    const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
    const todos = ref(todosData);
    function addTodo () {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value
        });
        newTodo.value = '';
      }
      saveData();
    }
    function removeTodo (index) {
      todos.value.splice(index, 1);
      saveData();
    }
    watch(newTodo, (str)=>{
      if(str.length > 20){
        return newTodo.value = ''
      }
    })
    function saveData () {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem('todos', storageData);
    }
    return {
      todos,
      newTodo,
      addTodo,
      removeTodo,
      saveData
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  list-style-type: none;
}
html {
  font-family: sans-serif;
}
h1,h2{
  color: white;
  text-align: center;
}
form{
  display: flex;
  justify-content: center;
}
.container{
  display: flex;
  justify-content: space-evenly;
  background-color:cyan;
}
div{
  background: rgb(63,94,251);
  background: radial-gradient(circle, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%);
  min-height: 600px;
  padding:5px
}
main{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -240%);
  width: 200px;
  display: flex;
  justify-content: space-around;
  padding: 30px;
  background-color: white;
  border-radius: 20px;
}
.noContent{
  display: none;
}
article{
  padding:5px;
  width:400px;
  border: whitesmoke solid 2px;
}
h4{
  text-align: center;
  color:white;
}
ul{
  display:flex;
  flex-direction: column;
  align-items: center;
}
li{
  margin: 5px 0;
  background-color: white;
  padding: 5px;
  border: purple solid 1px;
  border-radius: 5px;
  min-width: 260px;
  display: flex;
  justify-content: space-between;
}
button{
  background-color:red;
  color:white;
  border-radius: 4px;
  cursor: pointer;
  opacity: 0.7;
  transition:opacity 0.8ms;
}
button:hover{
  opacity: 1;
}

</style>