<template>
  <div id="burger-table">
    <Mensagem :msg="msg" v-show="msg" id="mensagemAtt" />
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#</div>
        <div>Cliente:</div>
        <div>Pao:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
      <div id="burger-table-rows">
        <div
          class="burger-table-row"
          v-for="burger in burgers"
          :key="burger.id"
        >
          <div class="order-number">{{ burger.id }}</div>
          <div>{{ burger.nome }}</div>
          <div>{{ burger.pao }}</div>
          <div>{{ burger.carne }}</div>
          <div>
            <ul id="opt-list">
              <li v-for="(opcional, index) in burger.opcionais" :key="index">
                {{ opcional }}
              </li>
            </ul>
          </div>
          <div>
            <select
              name="status"
              class="status"
              @change="attStatus($event, burger.id)"
            >
              <option
                v-for="statu in status"
                :key="statu.id"
                :value="statu.tipo"
                :selected="burger.status == statu.tipo"
              >
                {{ statu.tipo }}
              </option>
            </select>
            <button class="delete-btn" @click="deletarBurger(burger.id)">
              Apagar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Mensagem from "../components/Mensagem.vue";

export default {
  name: "Dashboard",
  data() {
    return {
      nomes: "",
      ids: "",
      carnes: "",
      paes: "",
      opcionais: [],
      burgers: null,
      burger_id: null,
      status: "",
      msg: "",
    };
  },
  components: {
    Mensagem,
  },
  methods: {
    async carregarBurgers() {
      const req = await fetch("http://localhost:3000/burgers");
      const data = await req.json();
      this.burgers = data;

      this.carregarStatus();
    },
    async carregarStatus() {
      const req = await fetch("http://localhost:3000/status");
      const data = await req.json();

      this.status = data;
    },
    async deletarBurger(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE",
      });

      const res = await req.json();

      this.carregarBurgers();
    },
    async attStatus(e, id) {
      const option = e.target.value;

      const optionJSON = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: optionJSON,
      });

      this.msg = "Pedido atualizado";
      setTimeout(() => (this.msg = ""), 3000);
    },
  },
  mounted() {
    this.carregarBurgers();
  },
};
</script>

<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}
#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
}
#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}
.burger-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}
#burger-table-heading div,
.burger-table-row div {
  width: 19%;
}
#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

#opt-list li {
  list-style: none;
  margin-left: 10px;
}

select {
  padding: 6px 6px;
  margin-right: 12px;
  height: 30px;
}
.delete-btn {
  height: 30px;
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 5px;
  font-size: 16px;
  margin: 5px auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}

#mensagemAtt {
  justify-content: space-between;
  align-items: center;
  margin-right: 0;
}
</style>
