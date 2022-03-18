<template>
  <div>
    <p>Componente de mensagem</p>
    <form id="burger-form">
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
          <option value="">Integral</option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha a carne do seu Burger:</label>
        <select name="carne" id="carne" v-model="carne" v-pre>
          <option value="">Selecione o tipo de carne</option>
          <option value="maminha">maminha</option>
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
            value="salame"
          />
          <span> salame</span>
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
      status: "Solicitado",
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
