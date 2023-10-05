<script setup>
import { computed, ref, watch } from "vue"
import { uid } from "uid"
import { Icon } from '@iconify/vue';
import TodoCreator from "../components/TodoCreator.vue"
import TodoItem from "../components/TodoItem.vue"

const todoList = ref([]);

const loadTodoList = () => {
  const data = JSON.parse(localStorage.getItem('todoList'));
  console.log(data);
  if (data) {
    todoList.value = data;
  }
};

const saveTodoList = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value));
};

loadTodoList();

watch(todoList, (newVal, prevVal) => {
  saveTodoList();
}, { deep: true });

const allTodoCompleted = computed(() => {
  return todoList.value.every(x => x.isCompleted);
});

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false
  });
};

const toggleTodoComplete = (index) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted;
};

const toggleEditTodo = (index, value) => {
  todoList.value[index].isEditing = !todoList.value[index].isEditing;
  if (todoList.value[index].isEditing === false) {
    todoList.value[index].todo = value;
  }
};

const deleteTodo = (index) => {
  todoList.value.splice(index, 1);
}
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo"></TodoCreator>
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem v-for="(todo, index) in todoList" :todo="todo" :index="index" @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo" @delete-todo="deleteTodo"></TodoItem>
    </ul>
    <p class="todos-msg" v-else>
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>You have no todos to complete, add one!</span>
    </p>
    <p v-if="allTodoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your todos!</span>
    </p>
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