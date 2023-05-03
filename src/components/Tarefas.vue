<template>
  <div>
    <h2>Tarefas</h2>
    <ul>
      <li v-for="(tarefa) in tarefas" :key="tarefa.id">
        {{ tarefa.responsavel }} - {{ tarefa.descricao }} - {{ tarefa.nivel }} - {{ tarefa.situacao }} - {{ tarefa.prioridade }}
        <button @click="handleDelete(tarefa.id)">Excluir</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      tarefas: [],
    };
  },
  mounted() {
    this.getTarefas();
  },
  methods: {
    getTarefas() {
      axios.get('https://api-tasks-fastapi-production.up.railway.app/tarefas').then((response) => {
        this.tarefas = response.data;
      });
    },
    handleDelete(id) {
      axios.delete(`https://api-tasks-fastapi-production.up.railway.app/deletar/${id}`).then(() => {
        this.getTarefas();
      });
    },
  },
};
</script>
