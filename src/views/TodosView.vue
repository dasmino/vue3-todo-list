<script setup>
import { ref } from 'vue'
import { uid } from "uid";
import TodoCreator from "../components/TodoCreator.vue";
import TodoItem from "../components/TodoItem.vue";

const todoList = ref([]);

const fetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

fetchTodoList();

const setTodoListLocalStorage = () => {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
};

const createTodo  = (todo)  => {
    todoList.value.push({
        id: uid(),
        todo,
        isCompleted: false,
        isEditing: null
    });
    setTodoListLocalStorage();
}

const deleteTodo = (todo) => {
    todoList.value = todoList.value.filter(
        (todoFilter) => todoFilter.id !== todo.id
    );
    setTodoListLocalStorage();
}

const toggleComplete = (index) => {
   todoList.value[index].isCompleted = !todoList.value[index].isCompleted;
    setTodoListLocalStorage();
}

const editTodo = (index) => {
   todoList.value[index].isEditing = !todoList.value[index].isEditing;
}

const updateTodo = (val, index) => {
    todoList.value[index].todo = val
    setTodoListLocalStorage();
};

</script>

<template>
    <main>
        <h1>Create Todo</h1>
        <TodoCreator @create-todo="createTodo"/>
         <ul class="todo-list">
            <TodoItem v-for="(todo, index) in todoList" 
                :todo="todo"
                 :index="index"
                 @delete-todo="deleteTodo"
                 @toggle-complete="toggleComplete"
                 @edit-todo="editTodo"
                 @update-todo="updateTodo"
             />
        </ul>
    </main>
</template>


<style scoped lang="scss">
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
    padding: 0px;
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