<template>
  <div class="container">
    <h2 class="texte-center mt-5">iBlue To-do List</h2>

    <!--Entradas-->
    <div class="d-flex">
      <input v-model="tarefa" type="text" placeholder="Inserir tarefa" class="form-control">
      <button @click="submitTarefa" class="btn btn-primary rounded-0">INSERIR</button>
    </div>

    <!--Tabela da lista de tarefas-->
    <table class="table table-bordered mt-5">
      <thead>
        <tr class="bg-primary text-white">
          <th scope="col" class="text-center">TAREFAS</th>
          <th scope="col" class="text-center">STATUS</th>
          <th scope="col" class="text-center">ALTERAR</th>
          <th scope="col" class="text-center">EXCLUIR</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(tarefa, index) in tarefas" :key="index">
          <td>
            <span :class="{'Feito': tarefa.status === 'Feito'}">{{tarefa.nome}}</span>
          </td>
          <td style="width: 120px">
            <span @click="changeStatus(index)" class="pointer"
              :class="{'text-danger': tarefa.status === 'À fazer',
              'text-warning': tarefa.status === 'Fazendo',
              'text-primary': tarefa.status === 'Feito'
              }"
            >
              {{tarefa.status}}
            </span>
          </td>
          <td>
            <!--Ícone de editar tarefa-->
            <div class="text-center" @click="editTarefa(index)">
              <span class="fa fa-pen pointer"></span>
            </div>
          </td>
          <td>
            <!--Ícone de excluir tarefa-->
            <div class="text-center" @click="deleteTarefa(index)">
              <span class="fa fa-trash pointer"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
    <footer>
      <p>Desenvolvido por Thiago de Oliveira Aires</p>    
    </footer>
  </div>
</template>

<script>

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data() {
    return {
      tarefa: '',
      editedTarefa: null,
      availableStatuses: ['À fazer', 'Fazendo', 'Feito'],
      /* Tarefas pré-incluídas */
      tarefas: [
        {
          nome: 'Assistir Aula de Node.js amanhã',
          status: 'À fazer'
        },
        {
          nome: 'Continuar escrita do TCC',
          status: 'Fazendo'
        }
      ]
    }
  },

  methods: {
    submitTarefa(){
      if(this.tarefa.length === 0) return;

      if(this.editedTarefa === null){
        this.tarefas.push({
          nome: this.tarefa,
          status: 'À fazer'
        });
      }else{
        this.tarefas[this.editedTarefa].nome = this.tarefa;
        this.editedTarefa = null;
      }

      this.tarefa = '';
    },

    deleteTarefa(index){
      this.tarefas.splice(index, 1);
    },

    editTarefa(index){
      this.tarefa = this.tarefas[index].nome;
      this.editedTarefa = index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tarefas[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tarefas[index].status = this.availableStatuses[newIndex];
    }
  }
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.Feito {
  text-decoration: double line-through rgb(75, 75, 250);
}
footer{
  bottom: 0;
  left: 0;
  position: fixed;
  width: 100%;
  background-color: #000;
  color: #fff;
  text-align: center;
}
</style>
