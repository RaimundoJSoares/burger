<template>
  <div>
    <form id="burger-form" @submit="createBurger">
      <div class="input-container">
        <label for="nome">Nome do Cliente:</label>
        <input
          type="text"
          id="nome"
          name="nome"
          v-model="nome"
          placeholder="Digite o seu nome"
        />
      </div>

      <div class="input-container">
        <label for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao" v-pre>
          <option value="">Selecione seu pão</option>
          <option v-bind:value="pao.tipo" v-for="pao in paes" v-bind:key="pao.id"> Integral </option>
           <option v-bind:value="pao.tipo" v-for="pao in paes" v-bind:key="pao.id"> 3 queijos </option>
            <option v-bind:value="pao.tipo" v-for="pao in paes" v-bind:key="pao.id"> Italiano Branco </option>
        </select>
        
      </div>
      <div class="input-container">
        <label for="carne">Escolha a carne do seu Burger:</label>
        <select name="carne" id="carne" v-model="carne" v-pre>
          <option value="">Selecione o tipo de carne</option>
          <option value="maminha">maminha</option>
          <option value="maminha">Alcatra</option>
          <option value="maminha">Picanha</option>
        </select>
      </div>

      <div id="opcionais-container" class="input-container">
        <label id="opctionais-title" for="opctionais"
          >Selecione os opcionais:</label
        >
        <div class="checkbox-container">
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            value="bacon"
          />
          <span> bacon</span>
        </div>

        <div class="checkbox-container">
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            value="tomate"
          />
          <span> tomate</span>
        </div>

        <div class="checkbox-container">
          <input
            type="checkbox"
            name="opcionais"
            v-model="opcionais"
            value="salame"
          />
          <span> salame</span>
        </div>
      </div>
      <div>
        <input type="submit" class="submit-btn" value="Criar meu Burger" />
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "BurgerForm",
  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata:null,
      nome: null,
      pao: null,
      carne: null,
      opcionais:[],
      msg: null,
    }
  },
  methods: {
    async getIngredientes() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault()
      
      const data = {
        nome: this.nome,
        carne : this.carne,
        pao: this.pao,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado"
      }

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: {"Content-type" :  "application/json"},
        body: dataJson
      })

      const res = await req.json()
      console.log(res)

      //limpar campos
      this.nome = "",
      this.carne = "",
      this.pao = "", 
      this.opcionais = ""
    }
  },
  mounted() {
    this.getIngredientes()
  }
};
</script>

<style scoped>
  #burger-form {
    max-width: 400px;
    margin: 0 auto;
    }

  .input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 15px;
  }
  label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
  }

  input,
  select {
    padding: 5px 10px;
    width: 300px;
  }

  #opcionais-container {
    flex-direction: row;
    flex-wrap: wrap;
  }

  #opctionais-title {
    width: 100%;
  }

  #chekbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
  }
  .checkbox-container span,
  .checkbox-container input {
    width: auto;
  }

  .checkbox-container span {
    margin-right: 20px;
    font-weight: bold;
  }

  .submit-btn {
    background-color: #fcba03;
    color: #222;
    font-weight: bold;
    border: 2px solid #fcba03;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: 0.7s;
    border-radius: 10px;
  }

  .submit-btn:hover {
    background-color: rgb(224, 179, 65);
  }
</style>
