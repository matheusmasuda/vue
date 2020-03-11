<template>
  <div>
    
    <titulo texto="Aluno"/>
    <div>
      <input
      type="text"
      placeholder="Nome do Aluno"
      v-model="nome"
      @keyup.enter="addAluno()"
    />
    <button type="button" class="btn btn_Input" @click="addAluno()">Salvar</button>
    </div>
    
    <table >
      <thead>
        <th>Mat</th>
        <th>Nome</th>
        <th>Opções</th>
      </thead>
      <tbody v-if="alunos.length">
        <tr v-for="(aluno, index) in alunos" :key="index" >
          <td>{{aluno.id}}</td>
          <td>{{aluno.nome}} {{aluno.sobrenome}}</td>
          
          <td><button class="btn btn_Danger" @click="remover(aluno)">Remover</button></td>
        </tr>
      </tbody>
      <tfoot v-if="alunos.length < 1">
        <p>Não há alunos cadastrados</p>
      </tfoot>
    </table>
  </div>
</template>

<script>
import Titulo from '../_shared/Titulo'; //importando o arquivo titulo.vue

export default {
  components: { //utilizando o componente de título 
    Titulo
  },
  data() {
    return {
      titulo: "Aluno",
      nome: "",
      alunos: []
    };
  },

  created(){ //utilizada para chamar o banco.json ao criar a estrutura do vue e listar os dados vindos do banco
    this.$http 
    .get('http://localhost:3000/alunos')
    .then(res => res.json())
    .then(alunos => this.alunos = alunos)
  },


  props: {},

  methods: {
    addAluno() {
      let _aluno = {
        nome: this.nome,
        sobrenome: ''
      }
      this.$http //nesse post fazemos a inserção do novo aluno no banco json
      .post('http://localhost:3000/alunos', _aluno)
      .then(res => res.json())
      .then (alunoRetornado => {
      this.alunos.push(alunoRetornado);
      this.nome = ''
      })
      
    },

    remover(aluno){
      this.$http
      .delete(`http://localhost:3000/alunos/${aluno.id}`)
      .then(() => {
        let indice = this.alunos.indexOf(aluno);
        this.alunos.splice(indice,1);
      })
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input{ 
  
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  color: #687f7f;
  display: inline; /*alinha os elementos na mesma linha, não necessitando de width*/
}
.btn_Input{
  width: 150px;
  border: 0px; /*nao quero que o botão tenha borda*/
  padding: 20px;
  font-size: 1.3em;
  background-color: rgb(116, 115, 115);
}
.btn_Input:hover{
  padding: 20px;
  margin: 0px;
  border: 0;
}
</style>
