<script setup>
import { defineEmits, reactive, ref } from 'vue'
import TodoButton from './TodoButton.vue';

const emit = defineEmits(['create-todo'])

const todoState = reactive({
    newTodo: '',
    invalid: null,
    errorMsg: ''
})

const createTodo = () => {
    todoState.invalid = null;
    if (todoState.newTodo !== '') {
        emit('create-todo', todoState.newTodo);
        todoState.newTodo = '';
        return;
    }
    todoState.invalid = true;
    todoState.errorMsg = 'Todo value can not be empty.';
};
</script>

<template>
    <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
        <input type="text" v-model="todoState.newTodo">
        <TodoButton @click="createTodo()">
        </TodoButton>
    </div>
    <p v-if="todoState.invalid" class="err-msg">{{ todoState.errorMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
    display: flex;
    transition: 250ms ease;
    border: 2px solid #41b080;

    &.input-err {
        border-color: red;
    }

    &:focus-within {
        box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
            0 -2px 4px -2px rgb(0 0 0 / 0.1);
    }

    input {
        width: 100%;
        padding: 8px 6px;
        border: none;

        &:focus {
            outline: none;
        }
    }
}

.err-msg {
    margin-top: 6px;
    font-size: 12px;
    text-align: center;
    color: red;
}
</style>