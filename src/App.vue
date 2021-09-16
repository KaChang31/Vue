<script setup lang="ts">
  import { useField, useForm } from 'vee-validate';
  import * as yup from 'yup'


  const schema = yup.object({
    email:yup.string().email().required(), 
    password:yup.string().required().min(10)
  });
 
  const munculAlert = () => {
    alert('INput')
  };

 const {setValues,meta, errors} = useForm({
   validationSchema: schema
 })

 const { value: email , meta:metaEmail} = useField('email');
 const { value: password ,meta:metaPassword} = useField('password');

</script>

<template>
<div class="container">
  <div class="row">
    <div class="col">
      <h3 class="mt-5 mb-3 text-center">Validasi &amp; Input Mask</h3>
    </div>

  </div>
  <form role="form">
    <div class="row">
      <div class="col-md-6">
        <div class="mb-6">
          <label for="#email" class="form-label">Email Address</label>
          <input type="email"  v-model="email" class="form-control form-control-lg" id="email" placeholder="Masukan Email ">
          <span>{{ errors.email }}</span>
        </div>
        <div class="md-3">
          <label for="password" class="form-label">Password</label>
          <input type="text" v-model="password" class="form-control form-control-lg" id="password" placeholder="Password">
          <span>{{ errors.password }}</span>
        </div>
        <div class="mb-3">
          <button class="btn btn-lg btn-primary shadow" :disabled= "!meta.valid" @click="munculAlert()">LOGIN</button>
        </div>
      </div>
    </div>
  </form>
</div>
</template>

<style scoped>
.selesai {
  text-decoration: line-through;
}
</style>