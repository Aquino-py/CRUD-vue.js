<template>
  <div>
    <Titulo :texto="`Aluno: ${aluno.nome}`" :btnVoltar="false">
      <button v-show="visualizando" class="btn btn-editar" @click="editar">Editar</button>
    </Titulo>
    <table>
      <tbody>
        <tr>
          <td class="colPequeno">Matrícula: </td>
          <td>
            <label>{{ aluno.id }}</label>
          </td>
        </tr>
        <tr>
          <td class="colPequeno">Nome: </td>
          <td>
            <label v-if="visualizando">{{ aluno.nome }}</label>
            <input v-else v-model="aluno.nome" type="text"/>
          </td>
        </tr>
        <tr>
          <td class="colPequeno">Sobrenome: </td>
          <td>
            <label v-if="visualizando">{{ aluno.sobrenome }}</label>
            <input v-else v-model="aluno.sobrenome" type="text"/>
          </td>
        </tr>
        <tr>
          <td class="colPequeno">Data nascimento: </td>
          <td>
            <label v-if="visualizando">{{ aluno.dataNasc }}</label>
            <input v-else v-model="aluno.dataNasc" type="text"/>
          </td>
        </tr>
        <tr>
          <td class="colPequeno">Professor: </td>
          <td>
            <label v-if="visualizando">{{ aluno.professor.nome }}</label>
            <select v-else v-model="aluno.professor">
              <option
                v-for="(professor, index) in professores"
                :key="index"
                :value="professor"
              >
              <span>{{ professor.nome }}</span>
              </option>
            </select> 
          </td>
        </tr>
      </tbody>
    </table>

    <div style="margin-top: 10px">
      <div v-if="!visualizando">
        <button class="btn btn-salvar" @click="salvar(aluno)">Salvar</button>
        <button class="btn btn-cancelar" @click="cancelar">Cancelar</button>
      </div>  
    </div>

  </div>
</template>

<script>
import Titulo from '../_share/Titulo'

export default {
  components: {
    Titulo
  },
  data() {
    return {
      professores: [],
      aluno: {},
      idAluno: this.$route.params.id,
      visualizando: true
    }
  },
  methods: {
    editar() {
      this.visualizando = !this.visualizando
    },
    salvar(_aluno) {
      let _alunoEditar = {
        id: _aluno.id,
        nome: _aluno.nome,
        sobrenome: _aluno.sobrenome,
        dataNasc: _aluno.dataNasc,
        professor: _aluno.professor
      }
      this.$http
        .put(`http://localhost:3000/alunos/${_alunoEditar.id}`, _alunoEditar)
      this.visualizando = true
    },
    cancelar() {
      this.visualizando = true
    }
  },
  created() {
    this.$http
      .get('http://localhost:3000/alunos/' + this.idAluno)
      .then(res => res.json())
      .then(aluno => this.aluno = aluno)
    this.$http
        .get('http://localhost:3000/professores')
        .then(res => res.json())
        .then(professor => this.professores = professor)

  }
}

    Titulo</script>

<style scoped>
.colPequeno {
  width: 20%;
}
input, select {
  margin: 0px;
  padding: 5px 10px;
  font-size: 0.9em;
  border: 1px solid silver;
  border-radius: 6px;
  font-family: Montserrat;
  width: 95%;
  background-color: rgb(245, 245, 245);
}
select {
  height: 38px;
  width: 100%;
}
.btn-editar {
  float: right;
  background-color: rgb(76, 186, 249);
}
.btn-salvar {
  float: right;
  background-color: rgb(79, 196, 68);
  margin-left: 10px;
}
.btn-cancelar {
  float: right;
  background-color: rgb(249, 186, 92);
}

</style>