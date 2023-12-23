<script setup>
   import { reactive } from 'vue';

   const estado = reactive({
      filtro: 'todas',
      tarefas:[
         {
            titulo: 'Estudar ES6',
            finalizada: false,
         },
         {
            titulo: 'Estudar SASS',
            finalizada: false,
         },
         {
            titulo: 'Estudar Vue-js',
            finalizada: true,
         },
      ],
      inputValue: ''
   });
   
   const checkTarefasPendentes = () =>{
      return estado.tarefas.filter(tarefa => !tarefa.finalizada)
   }
   const checkTarefasFinalizadas = () =>{
      return estado.tarefas.filter(tarefa => tarefa.finalizada)
   }

   const filtrarTarefas = ()=>{
      const { filtro } = estado;

      switch(filtro){
         case 'pendentes':
            return checkTarefasPendentes();
         case 'finalizadas':
            return checkTarefasFinalizadas();
         default: 
            return estado.tarefas;
      }
   }

   function adicionarTarefa() {
      estado.tarefas.push({
         titulo: estado.inputValue,
         finalizada: false,
      });
      estado.inputValue = '';
   }

</script>

<template>
   <div class="container">
      <header class="p-5 mb-4 mt-4 bg-light rounded-3">
         <h1>Minhas tareas</h1>
         <p>Você possui <span>{{ checkTarefasPendentes().length }}</span> tarefas pendentes</p>
      </header>
      <form @submit.prevent="adicionarTarefa">
         <div class="row">
            <div class="col">
               <input :value="estado.inputValue" @keyup="event => estado.inputValue = event.target.value" type="text" placeholder="Descrição da tarefa" class="form-control">
            </div>
            <div class="col-md-2">
               <button v-if="estado.inputValue !== ''" type="submit" class="btn btn-primary">Cadastrar</button>
               <button v-else type="submit" class="btn btn-primary" disabled>Cadastrar</button>
            </div>
            <div class="col-md-2">
               <select @change="event => estado.filtro = event.target.value" class="form-control">
                  <option value="todas">Todas as tarefas</option>
                  <option value="pendentes">Pendentes</option>
                  <option value="finalizadas">Fonalizadas</option>
               </select>
            </div>
         </div>
      </form>
      <ul class="list-group mt-4" v-for="tarefa in filtrarTarefas()">
         <li class="list-group-item">
            <input :id="tarefa.titulo" @change="event => tarefa.finalizada = event.target.checked" :checked="tarefa.finalizada" type="checkbox">
            <label :class="{done : tarefa.finalizada}" :for="tarefa.titulo" class="ms-3">
               {{ tarefa.titulo }}
            </label>
         </li>
      </ul>
   </div>
</template>

<style>
   .done{
      text-decoration: line-through;
   }
</style>
