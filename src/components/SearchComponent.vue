<template class="ml-2" id="app">
  <div class="mx-2">
    <h5 class="my-6 flex justify-center">{{ title }}</h5>
    <div class="sm:flex items-end justify-center">
      <div class="sm:flex justify-center">
        <div>
          <label class="label">
            <span class="label-text">Nome</span>
          </label>
          <input
            type="text"
            v-model="nome"
            placeholder="Digite seu nome"
            class="input input-bordered input-accent w-full md:w-auto"
          />
        </div>
        <div class="sm:mx-3">
          <label class="label">
            <span class="label-text">Sexo (opcional)</span>
          </label>
          <select
            v-model="sexySelected"
            class="select select-accent w-full md:w-auto"
          >
            <!-- <option selected>Ambos os sexos</option> -->
            <option v-for="option in sex" :key="option">
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
    <div class="my-3">
      <StateComponent v-if="load" :dataChart="periodValue" />
    </div>
    <ChartPeriod v-if="load" :dataChart="periodValue" />
  </div>
</template>

<script>
import ChartPeriod from "./ChartPeriod.vue";
import StateComponent from "./StateComponent.vue";

import axios from "axios";

export default {
  name: "SearchComponent",
  components: {
    ChartPeriod,
    StateComponent
  },
  data() {
    let periodValue;
    return {
      load: false,
      periodValue,
      title:
        "No Brasil, de acordo com o Censo Demográfico 2010, existem cerca de 200 milhões de habitantes com mais de 130 mil nomes diferentes.",
      sex: ["MASCULINO", "FEMININO"],
      nome: null,
      period: null,
      information: null,
      sexySelected: null,
      optionMF: null,
    };
  },
  // created() {
  //   this.getNameData();
  // },
  methods: {
    getNameData() {
      if (this.sexySelected == "MASCULINO") {
        this.optionMF = "M";
      } else {
        this.optionMF = "F";
      }
      
      if (this.nome.length > 2) {
        axios
          .get(
            `https://servicodados.ibge.gov.br/api/v2/censos/nomes/${this.nome}?sexo=${this.optionMF}`
          )
          .then((res) => {
            this.information = res.data;
            this.periodValue = res.data;
            console.log("GET", res.data);
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
