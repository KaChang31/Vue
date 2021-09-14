<script setup lang="ts">
  import { onMounted, reactive, ref } from 'vue'
  import Api from './services/api'
  type todoType = {
    id: number;
    nama: string;
    status: boolean;
  }
  const todoList = reactive<todoType[]>([])

  const todo = ref('');
  const idTodo = ref();
  let isEdit = ref();
  console.log(isEdit)
  let indexTodo = ref();

  
  const addTodo = async () => {
        const todoBody = {
        id:'',
        nama_task:todo.value,
        status_task:2 ,
      }
    try{
      const data = await Api.postResource('/task',todoBody,'POST')
      todo.value = '';
      
            const response = await fetch('http://localhost:8181');
            const sample = await response.json();
            const lastArr =  sample.slice(-1).pop();
            console.log(lastArr)

                todoList.push({
                  id: lastArr.id,
                  nama: lastArr.nama_task,
                  status: lastArr.status_task == 1 ? true : false
                })
      }
    catch(err){
    console.log(err)
    }
  }
  
  
  const editTodo = (i: number) => {
    //  cekEdit = true ;
    const row = todoList[i];
    todo.value = row.nama; 
    idTodo.value = row.id;
    let indexId = indexTodo.value[i];
    
    isEdit.value = true ; 
    console.log(isEdit.value)
    return row ; 
  }

  const simpanEdit = async (i:number) => {
     
     const todoBody = {
        id:'',
        nama_kegiatan:todo.value,
        status:2 ,
      }
     const y = todoList[i]; 
    
     const path = '/task' + '/' + y.id; 
     const data = await Api.putResource(path,todoBody,'PUT')
     todoList.splice(0,todoList.length); 
     const mintaData = await Api.getResource('/',todoList)
     todo.value = '';
     isEdit.value = false ; 
     }

  const deleteTodo = async (i: number) =>{
    const index = todoList[i]; 
    
    const path = '/task' + '/' + index.id;
    todoList.splice(0,todoList.length)
    const data = await Api.deleteResource(path,'DELETE');
    const get = await Api.getResource('/',todoList)
  }
 
  onMounted(async() =>{
    console.log(todoList)
    try {
      const data = await Api.getResource('/',todoList)
    }
    catch(err){
      console.log(err)
    }
  })

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
          <td><span :class="todo.status == true ? 'selesai' : 'blm-selesai'">{{ todo.nama }}</span></td>
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
        <button type="button" class="btn btn-primary mr-2" @click="isEdit = true ? simpanEdit() : addTodo() ">Simpan</button>
      </div>

      {{todoList}}
      <br>
      {{todo}}
      <br>
      {{isEdit}}

    </div>
  </div>
</div>

</template>

<style scoped>
.selesai {
  text-decoration: line-through;
}
</style>