
<script setup>
import { Icon } from '@iconify/vue';
import { uid } from 'uid';
import { ref,computed, onUpdated } from 'vue';
import TodoCreator from '@/components/TodoCreator.vue';
import TodoItem from '@/components/TodoItem.vue';

const todoList = ref([])

const todosCompleted = computed(() => {

    // console.log(todoList.value.every((todo) => todo. isCompleted));
    
    return todoList.value.every((todo) => todo.isCompleted);
});


const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList'));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
}

fetchTodoList();

const setTodoListLocalStorege = () => {
    localStorage.setItem('todoList', JSON.stringify(todoList.value));

}

const createTodo = (todo) => {
    todoList.value.push({
        id: uid(),
        todo,
        isCompleted: false,
        idEditing: null,
    });
    setTodoListLocalStorege();
}


const toggleEditTodo = (todoPos) => {
    todoList.value[todoPos].idEditing = !todoList.value[todoPos].idEditing;
    setTodoListLocalStorege()
}
const updateTodo = (todoVale, todoPos) => {
 todoList.value[todoPos].todo=todoVale
  setTodoListLocalStorege();
}

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted
  setTodoListLocalStorege()
}

const deletTodo = (todo) => {
    todoList.value = todoList.value.filter(
        (todoFilter) => todoFilter.id != todo.id);
setTodoListLocalStorege()
}


</script>


<template>
   <main>
<h1>Create  Todo</h1>
<TodoCreator  @create-todo="createTodo" >
    <template #button-content >Create</template>
</TodoCreator>

<ul class="todo-list" v-if="todoList.length>0">
<TodoItem
v-for="(todo,index) in  todoList"
:todo="todo"
:index="index"
:key="index"
@edit-todo="toggleEditTodo"
@update-todo="updateTodo"
@delete-todo="deletTodo"
@toggle-complete='toggleTodoComplete'
/>
</ul>



<p v-else class="todos-msg">
    <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>

</p>

<p v-if="todosCompleted && todoList.length>0" class="todos-msg">

 <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your todos!</span>
</p>




   </main>
</template>


<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>