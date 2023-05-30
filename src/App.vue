<template>
  <div id="App">

    <input type="text" placeholder="Convidados" v-model="Objeto.nome">
    <button class="button" v-if="Guardinha == 0" type="button" @click="adicionar()">Adicionar</button>
    <button class="button" v-if="Guardinha !== 0" type="button" @click="Alterar()">Salvar</button>
    <strong> Total de Convidados : {{ Vet.length }}</strong>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">N•</th>
          <th scope="col">Nome</th>
          <td></td>
          <td></td>
          <th scope="col">Controles</th>
        </tr>
      </thead>
      <tbody> 
        <tr v-for="(vet, i) in Vet">
          <th scope="row">{{ i + 1 }}</th>
          <td class="nome">{{ vet.nome }}</td>
          <td></td>
          <td></td>
          <td>
            <button type="button" @click="editar(vet._id, i)"><img src="
        https://cdn-icons-png.flaticon.com/512/3601/3601685.png" alt="Editar Pessoa" style="width: 19px;"></button>
          </td>

          <td>
            <button class="button" type="button" @click="deletar(vet._id, i)"><img
                src="https://e7.pngegg.com/pngimages/554/641/png-clipart-rubbish-bins-waste-paper-baskets-computer-icons-recycling-garbage-can-white-text.png"
                alt="Mandar para Lixeira" style=" margin: auto;   width: 19px;"></button>
          </td>
        </tr>

      </tbody>
    </table>
  </div>
</template>

<style scoped>
table {
  margin: 14px -47px -89px -2px;
}

#nome {
  display: flex;
  flex-direction: column;
  width: 70px;

}

#App {
  border-radius: 10px;
  width: 328px;
  margin: auto;
  margin-top: -324px;
}

input {
  padding: 8px;
  border-radius: 5px;
  text-align: center;

}

#button {
  background-color: #918d8d;
  color: white;
  font-size: 16px;
}

div {
  margin-top: -311px;
}

strong {
  display: block;
}
</style>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      Objeto: {},
      Vet: [],
      Guardinha: 0,
      Contador:0,
      Posicao: ""
    }
  },
  methods: {
    //Carregar Tela
    async CarregarTela() {
      let request = await axios.get('https://aplicacao-i.vercel.app/convidados')

      this.Vet = await request.data      
      this.Guardinha = 0
      this.Objeto.nome = ""
      console.log(this.Vet.nome)
    },
    //Adiconar Pessoas
    async adicionar() {
      console.log(this.Objeto.nome)
      let request = await axios.post('https://aplicacao-i.vercel.app/convidados/', this.Objeto)

      let Pessoa = await request.data

      this.Vet.push(Pessoa)
      this.CarregarTela()
    },


    async deletar(id, i) {
      await axios.delete("https://aplicacao-i.vercel.app/convidados/" + id)
      console.log(id)
      this.Vet.splice(i, 1)
      this.CarregarTela()
      this.Objeto.nome = ""
      console.log(this.Vet.nome)
    },
    editar(id, i) {
      let nome = this.Vet[i].nome
      this.Objeto.nome = nome
      this.Guardinha = 1
      this.Posicao = id
    },
    async Alterar() {
      await axios.patch('https://aplicacao-i.vercel.app/convidados/' + this.Posicao,this.Objeto)
      this.CarregarTela()
    },
  },
  async created() {
    await this.CarregarTela()
    console.log(this.Vet)
  },
  computed:{
    itemsFilstered(){
       return items
    }
  }
}
</script>

