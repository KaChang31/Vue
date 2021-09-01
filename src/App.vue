<script setup lang="ts">
  import { reactive, ref } from 'vue'
  type todoType = {
    id: number;
    nama: string;
    status: boolean;
  }
  const todoList = reactive<todoType[]>([
      { id: 1, nama: 'Belajar PHP', status: true },
      { id: 2, nama: 'Makan', status: false }
  ])

  const todo = ref('');
  const addTodo = () => {
    todoList.push({
      id: 0, nama: todo.value, status: false
    })
    todo.value = '';
  }

  const editTodo = (i: number) => {
    const row = todoList[i];
    todo.value = row.nama; 
  }

  const deleteTodo = (i: number) =>{
    todoList.splice(i, 1)
  }

</script>

<template>
<div class="container">
  <div class="row">
    <div class="col">
      <h3 class="text-center mt-5">Todo List</h3>
      <hr>
    
      <table class="table table-striped">
        <tr v-for="(todo, i) in todoList" :key="todo.id">
          <td>{{ i + 1 }}</td>
          <td>{{ todo.nama }}</td>
          <td>
            <input type="checkbox" v-model="todo.status">
          </td>
          <td class="text-end">
            <button type="button" class="btn-sm btn-info" @click="editTodo(i)">Edit</button>
            <button type="button" class="btn-sm btn-danger" @click="deleteTodo(i)">Hapus</button>
          </td>
        </tr>
      </table>

      <hr>

      <div class="d-flex">
        <input type="text" v-model="todo" class="form-control">
        <button type="button" class="btn btn-primary mr-2" @click="addTodo()">Simpan</button>
      </div>

      {{todoList}}
      <br>
      {{todo}}

    </div>
  </div>
</div>

</template>

