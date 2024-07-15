<template>
  <div>
    <v-row align="center" justify="center" class="mt-1 mb-0">
      <h3>Profit View of Company: {{ $props.selectedCompany }}</h3>
    </v-row>
    <div style="height: 90vh">
      <div id='myLinePlot' style="height: inherit"></div>
    </div>
  </div>
</template>

<script>
import Plotly from 'plotly.js/dist/plotly';

export default {
  name: "LinePlot",
  props: [
    "selectedCompany",
    "selectedAlgorithm",
  ],
  data() {
    return {
      LinePlotData: { x: [], y: [] }
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      try {
        // Request URL to retrieve single company from backend
        var reqUrl = 'http://127.0.0.1:5000/companies/' + this.$props.selectedCompany +'?algorithm=' + this.$props.selectedAlgorithm;
        // const reqUrl = 'http://127.0.0.1:5000/companies/1';
        console.log("ReqURL " + reqUrl);

        // Await response and data
        const response = await fetch(reqUrl);
        const responseData = await response.json();

        // Transform data to be usable by line plot
        responseData.profit.forEach((profit) => {
          this.LinePlotData.x.push(profit.year);
          this.LinePlotData.y.push(profit.value);
        });

        // Draw the line plot after the data is transformed
        this.drawLinePlot();
      } catch (error) {
        console.error("Error fetching data: ", error);
      }
    },
    drawLinePlot() {
      const layout = {};
      const config = { responsive: true, displayModeBar: false };
      const trace1 = {
        x: this.LinePlotData.x,
        y: this.LinePlotData.y,
        type: 'scatter'
      };
      const data = [trace1];
      Plotly.newPlot('myLinePlot', data, layout, config);
    }
  }
};
</script>

<style scoped>
/* Add any component-specific styles here */
</style>
