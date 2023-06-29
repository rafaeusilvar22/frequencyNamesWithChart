<template class="ml-2" id="app">
  <h5 class="my-6 flex justify-center">{{ title }}</h5>
  <div class="sm:flex grid items-end justify-center">
    <div class="sm:flex grid justify-center">
      <div>
        <label class="label">
          <span class="label-text">Nome</span>
        </label>
        <input
          type="text"
          v-model="nome"
          placeholder="Digite seu nome"
          class="input input-bordered input-accent w-full max-w-xs"
        />
      </div>
      <div class="sm:mx-3">
        <label class="label">
          <span class="label-text">Sexo (opcional)</span>
        </label>
        <select
          v-model="sexySelected"
          class="select select-accent w-full max-w-xs"
        >
          <!-- <option selected>Ambos os sexos</option> -->
          <option v-for="option in options" :key="option">
            {{ option }}
          </option>
        </select>
      </div>
    </div>
    <div class="sm:my-0 my-6">
      <button
        class="btn btn-outline btn-success w-full md:w-auto"
        @click="getNameData()"
      >
        Pesquisar
      </button>
    </div>
  </div>
  <div v-if="nome">
    <div class="my-6" v-for="info in information" :key="info">
      <span>{{ info.nome }}</span>
    </div>
  </div>
  <ChartPeriod v-if="load" :dataChart="periodValue" />
</template>

<script>
import ChartPeriod from "./ChartPeriod.vue";

import axios from "axios";

export default {
  name: "SearchComponent",
  components: {
    ChartPeriod,
  },
  data() {
    let periodValue;
    return {
      load: false,
      periodValue,
      title:
        "No Brasil, de acordo com o Censo Demográfico 2010, existem cerca de 200 milhões de habitantes com mais de 130 mil nomes diferentes.",
      message: "hello world",
      nome: "",
      period: "",
      information: null,
      sexySelected: "",
      options: ["M", "F"],
    };
  },
  // created() {
  //   this.getNameData();
  // },
  methods: {
    getNameData() {
      if (this.nome.length > 2) {
        axios
          .get(
            `https://servicodados.ibge.gov.br/api/v2/censos/nomes/${this.nome}?sexo=${this.sexySelected}`
          )
          .then((res) => {
            this.information = res.data;
            this.periodValue = res.data;
            console.log("GET", res.data);
            // console.log(
            //   this.periodValue[0].res.map(function (item) {
            //     return item.periodo;
            //   })
            // );
          })
          .catch((error) => {
            console.log(error);
          });
      }
      this.load = true;
    },
  },
};
</script>
