<template>
  <div>
    <form id="burger-form" @submit="enviaFormulario">
      <div class="input-container">
        <label for="nome">Informe seu nome:</label>
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
        <select name="pao" v-model="paoSelected">
          <option value="" disabled>Escolha seu pão</option>
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
            {{ pao.tipo }}
          </option>
        </select>
      </div>
      <div class="input-container">
        <label for="carne">Escolha a carne:</label>
        <select name="carne" :value="optSelected" v-model="carneSelected">
          <option value="" disabled>Escolha sua carne</option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
            {{ carne.tipo }}
          </option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label id="opcionais-title" for="opcionais"
          >Selecione os opcionais:</label
        >
        <div
          class="checkbox-container"
          v-for="opcional in opcionais"
          :key="opcional.id"
        >
          <input
            type="checkbox"
            name="opcionais"
            v-model="optSelected"
            :value="opcional.tipo"
          />
          <span>{{ opcional.tipo }}</span>
        </div>
      </div>

      <div class="input-container">
        <input class="submit-btn" type="submit" value="Montar meu Burger!" />
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "BurgerForm",
  data() {
    return {
      nome: "",
      paes: "",
      carnes: "",
      opcionais: "",
      paoSelected: "",
      carneSelected: "",
      optSelected: [],
    };
  },
  methods: {
    async carregarFormulario() {
      const requisicao = await fetch("http://localhost:3000/ingredientes");
      const data = await requisicao.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionais = data.opcionais;
    },
    async enviaFormulario(e) {
      e.preventDefault();

      const enviarDados = {
        nome: this.nome,
        paes: this.paoSelected,
        carnes: this.carneSelected,
        opcionais: Array.from(this.optSelected),
      };

      const dadosJson = JSON.stringify(enviarDados);

      const envioPost = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dadosJson,
      });
    },
  },
  mounted() {
    this.carregarFormulario();
  },
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
  margin-bottom: 20px;
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
#opcionais-title {
  width: 100%;
}
.checkbox-container {
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
  margin-left: 6px;
  font-weight: bold;
}
.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}
.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
