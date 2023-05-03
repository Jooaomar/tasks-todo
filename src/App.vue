<template>
    <h1>Adicionar Tarefa</h1>
    <form @submit.prevent="handleSubmit">
      <div>
        <label for="responsavel">Responsável:</label>
        <input type="text" id="responsavel" v-model="responsavel" />
      </div>
      <div>
        <label for="descricao">Descrição:</label>
        <textarea id="descricao" v-model="descricao"></textarea>
      </div>
      <div>
        <label for="nivel">Nível:</label>
        <select id="nivel" v-model="nivel">
          <option value="baixo">Baixo</option>
          <option value="médio">Médio</option>
          <option value="alto">Alto</option>
        </select>
      </div>
      <div>
        <label for="situacao">Situação:</label>
        <select id="situacao" v-model="situacao">
          <option value="pendente">Pendente</option>
          <option value="em-andamento">Em andamento</option>
          <option value="concluido">Concluído</option>
        </select>
      </div>
      <div>
        <label for="prioridade">Prioridade:</label>
        <input type="radio" id="prioridade-baixa" name="prioridade" value="baixa" v-model="prioridade" />
        <label for="prioridade-baixa">Baixa</label>
        <input type="radio" id="prioridade-media" name="prioridade" value="media" v-model="prioridade" />
        <label for="prioridade-media">Média</label>
        <input type="radio" id="prioridade-alta" name="prioridade" value="alta" v-model="prioridade" />
        <label for="prioridade-alta">Alta</label>
      </div>
      <button type="submit">Adicionar</button>
    </form>
    <!-- <ul>
      <li v-for="(tarefa, index) in tarefas" :key="index">
        {{ tarefa.responsavel }} - {{ tarefa.descricao }} - {{ tarefa.nivel }} - {{ tarefa.situacao }} - {{ tarefa.prioridade }}
        <button @click="handleDelete(index)">Excluir</button>
      </li>
    </ul> -->
    <Tarefas :tarefas="tarefas" @delete="deleteTarefa"/>
  </template>
  
  <script>
  import axios from 'axios';
  import Tarefas from './components/Tarefas.vue';

  export default {
    components: {
        Tarefas,
    },
    data() {
      return {
        responsavel: '',
        descricao: '',
        nivel: '',
        situacao: '',
        prioridade: '',
      }
    },
    methods: {
      handleSubmit() {
      const data = {
        responsavel: this.responsavel,
        descricao: this.descricao,
        nivel: this.nivel,
        situacao: this.situacao,
        prioridade: this.prioridade,
      }
      axios.post('https://api-tasks-fastapi-production.up.railway.app/adicionar/', data)
        .then(response => {
          console.log(response.data)
          this.tarefas.push(response.data)
          this.responsavel = ''
          this.descricao = ''
          this.nivel = ''
          this.situacao = ''
          this.prioridade = ''
          this.getTarefas()
        })
        .catch(error => {
          console.log(error.response.data)
        })
    },
    getTarefas() {
      axios.get('https://api-tasks-fastapi-production.up.railway.app/tarefas')
        .then(response => {
          this.tarefas = response.data
        })
        .catch(error => {
          console.log(error.response.data)
        })
    }
    },
    mounted() {
        this.getTarefas()
    },
}
</script>