<script setup>
import { reactive } from 'vue';
import Header from './components/Header.vue'
import Form from './components/Form.vue'
import List from './components/List.vue'

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar',
      finalizada: false,
    },
    {
      titulo: 'Dentista',
      finalizada: false,
    },
    {
      titulo: 'Academia',
      finalizada: true,
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
    <Header :tarefas-pendentes="getTarefasPendentes().length"></Header>
    <Form :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-trefa="cadastraTarefa"></Form>
    <List :tarefas="getTarefasFiltradas()"></List>
  </div>
</template>
