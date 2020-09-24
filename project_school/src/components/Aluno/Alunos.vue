<template>
  <div>
    <titulo texto="Aluno" />
    <div>
    <input type="text" placeholder="Nome do Aluno" v-model="nome" v-on:keyup.enter="addAluno()">
    <button class="btn btnInput" @click="addAluno()">Adicionar</button>
    </div>
      <table>
        <thead>
          <th>Mat.</th>
          <th>Nome</th>
          <th>opçoes</th>
        </thead>
        <tbody v-inf="alunos.length">
          <tr v-for="(aluno, index) in alunos" :key="index">
            <td>{{aluno.id}}</td>
            <td>{{aluno.nome}} {{aluno.sobrenome}}</td>
            <td>
              <button class="btn btn_Danger" @click="remover(aluno)">Remover</button>
            </td>
          </tr>
        </tbody>
        <tfoot v-if="!alunos.length">Nenhum Aluno encontrado</tfoot>
        
      </table>
  </div>
</template>

<script>
import Titulo from '../_share/Titulo.vue';

export default {
  components:{
    Titulo
  },
  data(){
    return{
      titulo: 'Aluno',
      nome: '',
      alunos: []
    }
  },
  
  created(){
    this.$http
      .get('http://localhost:3000/alunos')
      .then(res=> res.json())
      .then(alunos => this.alunos = alunos)
  },

  props:{ },

  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome:""
      }
      this.$http
      .post('http://localhost:3000/alunos', _aluno)
      .then(res=> res.json())
      .then(alunoretornado =>{
        this.alunos.push(alunoretornado);
        this.nome = '';
      })      

    
    },
    remover(aluno){
      this.$http
      .delete(`http://localhost:3000/alunos/${aluno.id}`)
      .then(() => {
        let indice = this.alunos.indexOf(aluno);
        this.alunos.splice(indice, 1);
      })
      
      
    }
  },
}
</script>

<style scoped>
input{
  border:0;
  padding: 20px;
  font-size: 1.3em;
  color: black;
  display: inline;
}
.btnInput{
  border: 0px;
  padding: 20px;
  font-size: 1.3em;
  background-color: gray;
  display: inline;
}

.btnInput:hover{
  padding: 20px;
  margin: 0px;
  border:0px;
}

</style>
