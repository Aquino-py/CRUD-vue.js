<template>
  <div>
    <Titulo :btnVoltar="true" :texto="professorId !== undefined ? 'Professor: ' + professor.nome : 'Todos os alunos'"/>
    <div v-if="professorId !== undefined">
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
          <td class="colPequeno" style="width: 10%">{{aluno.id}}</td>
          <router-link
            tag="td"
            :to="`/alunoDetalhe/${aluno.id}`"
            style="cursor: pointer"
          >
            {{aluno.nome}} {{ aluno.sobrenome }}
          </router-link>
          <td class="colPequeno" style="background-color: #e0edf4">
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
      professorId: this.$route.params.prof_id,
      professor: {},
      nome: '',
      alunos: []
    }
  },
  created() {
    if (this.professorId) {
      this.carregarProfessores()
      this.$http
      .get('http://localhost:3000/alunos?professor.id=' + this.professorId)
      .then(res => res.json())
      .then(alunos => this.alunos = alunos)

    } else {
      this.$http
      .get('http://localhost:3000/alunos')
      .then(res => res.json())
      .then(alunos => this.alunos = alunos)
    }
  },  
  props: {

  },
  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome: "",
        professor: {
          id: this.professor.id,
          nome: this.professor.nome,
        }
      }
      
      if(this.nome !== '') {
        this.$http
          .post('http://localhost:3000/alunos', _aluno)
          .then(res => res.json())
          .then(alunoRetornado => {
            this.alunos.push(alunoRetornado)
            this.nome = ''
          })
      }else {
        return alert('Erro ao adicionar o aluno!  Adicione um nome e tente novamente')
      }
      
    },
    remover(aluno) {
      this.$http
        .delete(`http://localhost:3000/alunos/${aluno.id}`)
        .then(() => {
          let index = this.alunos.indexOf(aluno)
          this.alunos.splice(index, 1)
        })
    },
    carregarProfessores() {
        this.$http
        .get('http://localhost:3000/professores/' + this.professorId)
        .then(res => res.json())
        .then(professor => {
          this.professor = professor
        })
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table tr td{
  text-align: center;
  vertical-align: middle;
}
</style>
