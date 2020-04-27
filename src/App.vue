<template>
  <div id="app">

    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">LISTA DE PRODUTOS</a>
      </div>
    </nav>

    <div class="container">



      <form @submit.prevent="salvar">

          <label>Nome</label>
          <input type="text" id="nome" placeholder="Nome" v-model="produto.nome" required name=nome>
          <label>Quantidade</label>
          <input type="number" id="qtde" placeholder="QTD" v-model="produto.quantidade" required name=qtde>
          <label>Valor</label>
          <input type="text" id="valor" placeholder="Valor" v-model="produto.valor" required name=valor>

          <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>

      </form>

      <table>

        <thead>

          <tr>
            <th>NOME</th>
            <th>QUANTIDADE</th>
            <th>VALOR</th>
            <th>OPÇÕES</th>
          </tr>

        </thead>

        <tbody>

          <tr v-for="produto of produtos" :key="produto.id">

            <td>{{ produto.nome }}</td>
            <td>{{ produto.quantidade }}</td>
            <td>{{ produto.valor }}</td>
            <td>
              <button @click="editar(produto)" class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button @click="remover(produto)" class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
            </td>

          </tr>

        </tbody>
      
      </table>

    </div>

  </div>
</template>

<script>

import Produto from './services/produtos'

export default {

    data(){
      return {
        produto: {
          id: '',
          nome: '',
          quantidade: '',
          valor: ''

        },
        produtos: [],
        errors: []
      }
    },

  mounted(){ 
      this.listar()

  },

  methods:{

    listar(){
      Produto.listar().then(resposta => {
        this.produtos = resposta.data
      })
    },

    salvar(){
      if(!this.produto.id){
        Produto.salvar(this.produto).then(resposta => {
          this.produto = {}
          alert('Salvo com Sucesso!')
          this.resposta = resposta
          this.listar();
          this.errors = []
        }).catch(e => {
          this.errors = e.response.data.errors
        })

      }else{
        Produto.atualizar(this.produto).then(resposta => {
          this.produto = {}
          alert('Atualizado com Sucesso!')
          this.resposta = resposta
          this.listar();
          this.errors = []
        }).catch(e => {
          this.errors = e.response.data.errors
        })
    }

    },

    editar(produto){
      this.produto = produto
    },

    remover(produto){
      if(confirm('Deseja excluir o produto?')){
        Produto.apagar(produto).then(resposta => {
          this.resposta = resposta
          this.listar()
          this.errors = {}
        }).catch(e => {
          this.errors = e.response.data.errors
        })
      }
    }
  }
}
</script>

<style>
</style>
