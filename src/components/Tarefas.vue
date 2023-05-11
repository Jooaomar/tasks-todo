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
      axios.get('https://django-server-production-61b8.up.railway.app/task_api/').then((response) => {
        this.tarefas = response.data;
      });
    },
    handleDelete(id) {
      axios.delete(`https://django-server-production-61b8.up.railway.app/task_api/${parseInt(id)}`).then(() => {
        this.getTarefas();
      });
    },
  },
};
</script>
