<template>
  <div>
    <Titulo texto="Professores" :btnVoltar="true"/>
    <table>
      <thead>
        <th>Cod.</th>
        <th>Nome</th>
        <th>Alunos</th>
      </thead>
      <tbody v-if="professores.length">
        <tr v-for="(professor, index) in professores" :key="index">
          <td class="colPequeno" style="width: 10%">{{professor.id}}</td>
            <router-link
              :to="`/alunos/${ professor.id }`"
              tag="td"
              style="cursor: pointer"
            >
              {{professor.nome}} {{ professor.sobrenome }}
            </router-link>
          <td class="colPequeno" style="width: 15%">
            {{ professor.qtdAlunos }}
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
  components: { Titulo },
    data() {
      return {
        professores: [],
        Alunos: []
      }
    },
    created() {
      this.$http
      .get('http://localhost:3000/alunos')
      .then(res => res.json())
      .then(alunos => {
        this.Alunos = alunos
        this.carregarProfessores()
      })
    },  
    props: {

    },
    methods: {
      pegaQtdAlunosPorProfessor() {
        this.professores.forEach((professor, index) => {
          professor = {
            id: professor.id,
            nome: professor.nome,
            qtdAlunos: this.Alunos.filter(aluno => 
              aluno.professor.id == professor.id
            ).length
          }
          this.professores[index] = professor
        })
      },
      carregarProfessores() {
        this.$http
        .get('http://localhost:3000/professores')
        .then(res => res.json())
        .then(professor => {
          this.professores = professor
          this.pegaQtdAlunosPorProfessor()
        })
      }
    },
  }
</script>

<style scoped>
table tr td{
  text-align: center;
  vertical-align: middle;
}

</style>