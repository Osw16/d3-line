<template>
  <div id="app">
    <h1>Using Vue 3 (Composition API) with D3</h1>
    <responsive-line-chart :factsData="loadData" />
    <div class="buttons">
      <!-- <button @click="addData">Add data</button> -->
      <!-- <button @click="filterData">Filter data</button> -->
      <button @click="fetchData">Fetch data</button>
    </div>
    <!-- <strong>Output:</strong>
      {{output}} -->
  </div>
</template>

<script>
import * as d3 from "d3";
import ResponsiveLineChart from "./components/ResponsiveLineChart.vue";

export default {
  name: "App",
  components: {
    ResponsiveLineChart,
  },
  data() {
    return {
      output: [],
      loadData: [],
      crime: 2,
      ent: 1,
      mun1: 1,
      testData: [1, 2, 3, 4, 5],
    };
  },
  methods: {
    addData() {
      // add random value from 0 to 50 to array
      this.loadData

    },

    filterData() {
      this.loadData = this.loadData.filter((v) => v <= 35);
    },

    async fetchData() {
      // console.log('fetchData method')

            const payload = {
        id_crime: Number(this.crime, 10),
        id_ent: Number(this.ent, 10),
        id_mun1: Number(this.mun1, 10),
      };
      // console.log(payload);
      let data = await d3.json(
        "https://spotlight-unfpa.datacivica.org/api/v1/timeline",
        {
          method: "POST",
          body: JSON.stringify(payload),
        }
      );
       this.loadData = data.map(d => ({year:d.year,total:+d.total_nac})) 
      
      // console.log(this.loadData)
   
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 720px;
  margin: 100px auto;
  padding: 0 20px;
}

svg {
  /* important for responsiveness */
  display: block;
  fill: none;
  stroke: none;
  width: 100%;
  height: 100%;
  overflow: visible;
  background: #eee;
}

.buttons {
  margin-top: 2rem;
}
</style>
