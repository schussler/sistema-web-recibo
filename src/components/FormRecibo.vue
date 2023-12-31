<template>
  <NavBar v-show="exibirform" />

  <div v-show="exibirform" class="container has-background-primary p-6">
    <div class="formulario">
      <!-- AREA TUTOR -->
      <div class="field">
        <label class="label">TUTOR</label>
        <div class="control">
          <input
            class="input"
            type="text"
            placeholder="DIGITE O NOME DO TUTOR"
            v-model="tutorRecibo"
          />
        </div>
      </div>
      <!-- AREA PRODUTOS -->
      <div class="field">
        <label class="label">PRODUTOS</label>

        <div class="control is-flex">
          <input
            class="input w-10p"
            type="number"
            placeholder="QTD"
            v-model="qtd"
            ref="input1"
          />
          <input
            class="input w-70p"
            type="text"
            placeholder="DIGITE O PRODUTO"
            v-model="produto"
          />
          <input
            class="input w-10p"
            type="number"
            placeholder="UNT"
            v-model="unt"
          />
        </div>
        <div class="btn_add p-2">
          <button
            @click="addItem"
            @keydown="verificarTecla"
            class="button is-info"
          >
            ADD
          </button>
          <!-- <button class="button is-warning w-20">DEL</button>
          <button class="button is-danger w-20">RESET</button> -->
        </div>
      </div>

      <!-- AREA TOTAL -->
      <div class="area_total">
        <div class="field"></div>
        <div class="btn_add p-2 is-flex is-justify-content-space-evenly">
          <button @click="ImprimirCupom" class="button is-info w-80">
            IMPRIMIR
          </button>
          <button @click="img" class="button is-info w-80">GERAR IMG</button>
          <!-- <button class="button is-warning w-20">DEL</button>
          <button class="button is-danger w-20">RESET</button> -->
        </div>
      </div>
    </div>
    <div>
      <h1>{{ tutorRecibo }}</h1>

      <div v-for="(item, index) in produtos" :key="index">
        <p>
          {{ item.qtd }} | {{ item.produto }} | {{ item.unt }} |
          {{ item.totalItem }}
        </p>
      </div>
      <p>total: {{ calcularSoma() }}</p>
    </div>
  </div>
  <div v-show="exibirCupom" class="cupom is-uppercase" id="cupom">
    <br />
    <div class="titulo has-text-weight-bold is-size-3">STUDIO PET GM</div>
    <div class="dados_loja">****** COMANDA DE SERVIÇO ******</div>

    <div class="dados_loja">
      av.Jose davi - 626 - cidade beira mar - RO <br />(22) 99848-6513 / (22)
      99931-5339
    </div>
    <div class="data">{{ formatarData() }}</div>

    <hr class="tutor" />
    <div class="dados_cliente">
      <p class="has-text-weight-bold">TUTOR</p>
      <p class="has-text-weight-bold">{{ tutorRecibo }}</p>
    </div>
    <hr class="tutor" />
    <table id="tabelaProdutos" class="tabela">
      <thead>
        <tr>
          <th class="w-8p">QTD</th>
          <th class="w-40p">SERVIÇO</th>
          <th class="w-15p">UNT</th>
          <th class="w-15p">TOTAL</th>
        </tr>
      </thead>
      <hr />

      <tbody v-for="(item, index) in produtos" :key="index">
        <tr class="mt-2">
          <td>{{ item.qtd }}</td>
          <td>{{ item.produto }}</td>
          <td>{{ item.unt }}</td>
          <td>R$ {{ item.totalItem }}</td>
        </tr>
        <!-- Linhas de produtos serão adicionadas aqui dinamicamente -->
      </tbody>
    </table>
    <hr />

    <div class="is-size-3 has-text-weight-bold" id="totalFinal">
      Total: R$ {{ calcularSoma() }}
    </div>
  </div>
</template>

<script>
import NavBar from "@/components/NavBarComponents.vue";
export default {
  components: {
    NavBar,
  },
  data() {
    return {
      tutorRecibo: "",
      qtd: "",
      produto: "",
      unt: "",
      totalItem: "",
      desconto: "",
      produtos: [],
      exibirCupom: false,
      exibirform: true,
    };
  },
  methods: {
    addItem() {
      if (this.qtd && this.produto && this.unt) {
        var Tqtd = parseFloat(this.qtd);
        var Tunt = parseFloat(this.unt);
        var totalItem = Tqtd * Tunt;
        this.produtos.push({
          qtd: this.qtd,
          produto: this.produto,
          unt: this.unt,
          totalItem: totalItem,
        });
      } else {
        alert("Preencha todos os campos");
      }
      this.qtd = "";
      this.produto = "";
      this.unt = "";
      // Chamar verificarTecla no final
      this.$refs.input1.focus();
    },
    calcularSoma() {
      // Inicializa uma variável para armazenar a soma
      let soma = 0;

      // Itera sobre os itens da lista e acumula os valores desejados
      for (let i = 0; i < this.produtos.length; i++) {
        soma += parseFloat(this.produtos[i].totalItem) || 0;
      }

      // Retorna o resultado da soma
      return soma;
    },
    formatarData() {
      const dataAtual = new Date();

      // Extrai as partes da data
      const dia = dataAtual.getDate();
      const mes = dataAtual.getMonth() + 1; // Lembrando que os meses são indexados de 0 a 11
      const ano = dataAtual.getFullYear();
      const hora = dataAtual.getHours();
      const minutos = dataAtual.getMinutes();

      // Adiciona um zero à esquerda se for menor que 10
      const diaFormatado = dia < 10 ? `0${dia}` : dia;
      const mesFormatado = mes < 10 ? `0${mes}` : mes;
      const horaFormatada = hora < 10 ? `0${hora}` : hora;
      const minutosFormatados = minutos < 10 ? `0${minutos}` : minutos;

      // Retorna a data formatada
      return `${diaFormatado}/${mesFormatado}/${ano} - ${horaFormatada}:${minutosFormatados}`;
    },
    async ImprimirCupom() {
      this.exibirCupom = true;
      this.exibirform = false;
      await this.$nextTick(); // Aguarda o próximo ciclo de atualização do Vue
      window.print();
      this.exibirCupom = false;
      this.exibirform = true;
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
}
hr {
  margin: 7px;
  border: none; /* Remova a borda padrão */
  height: 1px; /* Altura da linha */
  background-color: #333; /* Cor de fundo da linha */
}
.cupom {
  width: 58mm;
}

/* SELETORES */
input {
  text-align: center;
}
/* CONFIG GERAL */

.w-10p {
  width: 15%;
}
.w-150 {
  width: 150px;
}
.w-80 {
  width: 100px;
}
.w-70p {
  width: 70%;
}

.w-8p {
  width: 8%;
}

.w-40p {
  width: 40%;
}

.w-15p {
  width: 15%;
}
.bd-b {
  border: 1px solid black;
}
.bd-r {
  border: 1px solid red;
}
.bd-b {
  border: 1px solid black;
}
.bd-g {
  border: 1px solid black;
}

input[type="number"] {
  -moz-appearance: textfield; /* Firefox */
  appearance: textfield; /* Restante dos navegadores */
}
@media print {
  .tutor {
    margin: 7px;
    border: none;
    height: 1px;
  }

  .cupom {
    width: 80mm;
  }

  /* Adicione outros estilos específicos para a impressão conforme necessário */
}
</style>
