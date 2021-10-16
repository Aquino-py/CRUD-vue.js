<template>
  <div>
    <Titulo texto="Aluno"/>
    <div>
    <input
      type="text"
      placeholder="Nome do aluno"
      v-model="nome"
      @keyup.enter="addAluno()"
    />
    <button
      class="btn btn-input"
      @click="addAluno()"
    >
      Add
    </button>
    </div>
    <table>
      <thead>
        <th>Mat.</th>
        <th>Nome</th>
        <th>Opções</th>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="(aluno, index) in alunos" :key="index">
          <td>{{aluno.id}}</td>
          <td>{{aluno.nome}} {{ aluno.sobrenome }}</td>
          <td>
            <button class="btn btn-danger" @click="remover(aluno)">
              Remover
            </button>
          </td>
        </tr>
      </tbody>
      <tfoot v-else>
        Nenhum aluno encontrado...
      </tfoot>
    </table>

  </div>
</template>

<script>
import Titulo from '../_share/Titulo.vue'

export default {
  components: {
    Titulo
  },
  data() {
    return {
      nome: '',
      alunos: []
    }
  },
  created() {
    this.$http
    .get('http://localhost:3000/alunos')
    .then(res => res.json())
    .then(alunos => this.alunos = alunos)
  },  
  props: {

  },
  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome: ""
      }

      this.$http
        .post('http://localhost:3000/alunos', _aluno)
        .then(res => res.json())
        .then(alunoRetornado => {
          this.alunos.push(alunoRetornado)
          this.nome = ''
        })
      
    },
    remover(aluno) {
      this.$http
        .delete(`http://localhost:3000/alunos/${aluno.id}`)
        .then(() => {
          let index = this.alunos.indexOf(aluno)
          this.alunos.splice(index, 1)
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700");

body{
  background-color: #eee;
  font-family: "Montserrat", sans-serif;
  display: grid;
  justify-items: center;
}
body, html {
  margin: 0;
  height: 100%;
}

table {
  margin: 0px;
  padding: 0px;
  list-style-type: none;
  width: 100%;
}
table tr td{
  padding: 20px;
  font-size: 1.3em;
  background-color: #e0edf4;
  margin-bottom: 2px;
  color: #3e5252;
  text-align: center;
  vertical-align: middle;
}
table thead th{
  background-color: rgb(184, 208, 216) !important;
  font-size: 1.2em;
  padding: 10px 0px;
  text-align: center !important;
}
.colPequeno {
  width: 5%;
}
.btn-danger {
  background-color: #fa4430;
}
.btn {
  border: 1px solid #000;
  cursor: pointer;
  padding: 10px 15px;
  color: white;
  font-weight: bold;
  border-radius: 5px;
  border-bottom: 3px solid black;
  position: static;
}
.btn:hover {
  text-shadow: 1px 1px 1px black;
  margin-top: 3px;
  border-bottom: 1px solid black;
}
input {
  width: calc(100%-150px);
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline;
}
.btn-input {
  width: 150px;
  border: 0px;
  padding: 20px;
  font-size: 1.3em;
  background-color: rgb(116, 115, 155);
}
.btn-input:hover {
  padding: 20px;
  margin: 0px;
  border: 0px;
}
</style>
