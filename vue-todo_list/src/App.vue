<script setup>
  import { reactive } from 'vue';
  import CompHeader from './components/CompHeader.vue';
  import CompForm from './components/CompForm.vue';
  import CompList from './components/CompList.vue';
  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false
      },
      {
        titulo: 'Estudar TypeScript',
        finalizada: true
      },
      {
        titulo: 'Estudar ES6',
        finalizada: false,
      }
    ]
  })
  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }
  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }
  const getTarefasFiltradas = () => {
    const { filtro } = estado;
    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }
  const cadastraTarefa = () => {
   const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
   }
   estado.tarefas.push(tarefaNova);
   estado.tarefaTemp = '';
  }
</script>
<template>
  <div class="container">
    <CompHeader :tarefas-pendentes="getTarefasPendentes().length" />
    <CompForm 
      :tarefa-temp="estado.tarefaTemp"
      :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value"
      :cadastra-tarefa="cadastraTarefa"
      :trocar-filtro="evento => estado.filtro = evento.target.value" />
    <CompList :tarefas="getTarefasFiltradas()"/>
  </div>
</template>