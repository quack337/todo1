<script setup>
import { ref, onMounted } from 'vue';

const todos = ref([]);
const title = ref('');

const KEY = "todo1.todos";

onMounted(() => {
  const json = localStorage.getItem(KEY);
  if (json) todos.value = JSON.parse(json);
})

function addTodo() {
  const lastId = todos.value.length == 0 ? 0 :
    todos.value[todos.value.length - 1].id;
  todos.value.push({id: lastId + 1, title: title.value, done: false});
  localStorage.setItem(KEY, JSON.stringify(todos.value))
  title.value = "";
}

function deleteTodo(index) {
  if (confirm("삭제하시겠습니까?")) {
    todos.value.splice(index, 1);
    localStorage.setItem(KEY, JSON.stringify(todos.value))
  }
}
</script>

<template>
  <div>
    <h1>할 일</h1>
    <table>
      <thead>
        <tr><td>ID</td><td>할일</td></tr>
      </thead>
      <tbody>
        <tr v-for="(todo, index) in todos"
            v-bind:key="todo.id" v-bind:class="{gray: todo.done}">
          <td>{{ todo.id }}</td>
          <td>
            <input type="checkbox" v-model="todo.done" />
            {{ todo.title }}
            <span v-on:click="deleteTodo(index)">x</span>
          </td>
        </tr>
        <tr v-if="todos.length == 0">
          <td></td><td>할 일이 없습니다</td>
        </tr>
      </tbody>
    </table>
    <input type="text" v-model.trim="title" />
    <button type="button" v-on:click="addTodo">추가</button>
  </div>
</template>

<style scoped>
h1 { border-bottom: 1px solid gray; text-shadow: 1px 1px 2px #aaa;}
table { margin: 1em 0; width: 100%; font-size: 11pt; }
thead { background-color: #eee; text-align: center; }
td { border-bottom: 1px solid #bbb; padding: 0.3em; }
td:nth-child(1) { text-align: center; width: 30px;  }
td span { font-weight: bold; float: right; }
td span:hover { color: red; cursor: pointer; }
tr:has(span:hover) { background-color: #ffd; }
input[type=text] { padding: 0.3em; margin-right: 0.3em; width: 22em; font-size: 11pt; }
button { padding: .2em .9em .3em .9em; border: none; font-size: 11pt;
  background-color: #08f; box-shadow: 2px 2px 2px #aaa;
  color: white; text-shadow: 1px 1px 2px #444; }
button:active { box-shadow: none; transform: translateY(2px); border: none; }
tr.gray { background-color: #f8f8f8; color: #bbb; text-decoration: line-through; }
input[type=checkbox] { accent-color: #bbb; }
</style>
