<template>
  <div class="app">
    <div class="flex justify-center aling-center w-full md:w-auto">
      <apexchart
        class="chart"
        width="100%"
        type="area"
        :options="chartOptions"
        :series="series"
      ></apexchart>
    </div>
  </div>
</template>
<script>
import VueApexCharts from "vue3-apexcharts";
export default {
  components: {
    apexchart: VueApexCharts,
  },
  // As props nos permitem passar dados de um componente pai para um componente filho
  // por meio de atributos personalizados na tag HTML do componente pai.
  // https://www.koderhq.com/tutorial/vue/component-prop/
  props: ["dataChart"],
  data() {
    return {
      chartOptions: {
        colors: ["#F44336"],
        chart: {
          id: "vuechart-example",
          zoom: {
            enabled: false,
          },
        },
        dataLabels: {
          enabled: false,
        },
        stroke: {
          curve: "straight",
        },
        title: {
          text: "Nascimento por Décadas",
          align: "left",
          style: {
            fontSize: "16px",
            color: "#7AA49E",
          },
        },
        xaxis: {
          categories: [1930, 1949, 1950, 1960, 1970, 1980, 1990, 2000, 2010],
        },
      },
      series: [
        {
          name: "Popularidade:",
          data: [],
        },
      ],
    };
  },
  // todas as fases do ciclo de vida do Vue js 3 e seus hooks
  // https://www.youtube.com/watch?v=XpIhFIDtB1M&ab_channel=TiagoMatos
  beforeUpdate() {
    let newData = this.dataChart[0].res.map((item) => {
      return item.frequencia;
    });
    let newName = this.dataChart.map((item) => {
      return item.nome;
    });
    this.series = [
      {
        name: newName,
        data: newData,
      },
    ];
  },
};
</script>
<style>
.chart {
  width: 100%;
}
</style>
