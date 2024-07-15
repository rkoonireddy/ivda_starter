<template>
  <div>
      <v-container fluid>
      <v-row class="header">
        Example Project
      </v-row>
      <v-row>
        <v-col cols="12" md="2" class="sideBar">
          <v-row>
            <v-col cols="12" sm="12">
              <div class="control-panel-font">Company Overview</div>
            </v-col>
      </v-row>
      <v-row>
            <v-col cols="12" sm="12">
                <v-select
                    :items="categories.values"
                    label="Select a category"
                    dense
                    v-model="categories.selectedValue"
                    @change="changeCategory"
                ></v-select>
            </v-col>
      </v-row> 
      <v-row>
        <v-col cols="12" sm="12">
            <div class="control-panel-font">Profit View of Company</div>
        </v-col>
  </v-row>
  <v-row>
        <v-col cols="12" sm="12">
            <v-select
                :items="companies.values"
                label="Select a company"
                dense
                v-model="companies.selectedValue"
                @change="changeCompany"
            ></v-select>
        </v-col>
  </v-row>
  <v-row>
        <v-col cols="12" sm="12">
            <v-select
                :items="algorithm.values"
                label="Select an algorithm"
                dense
                v-model="algorithm.selectedValue"
                @change="changeAlgorithm"
            ></v-select>
        </v-col>
  </v-row>           
        </v-col>
        <v-col cols="12" md="5">
          <ScatterPlot :key="scatterPlotId" :selectedCategory="categories.selectedValue"/>
          @changeCurrentlySelectedCompany="changeCurrentlySelectedCompany"/>
        </v-col>
        <v-col cols="12" md="5">
          <LinePlot :key="linePlotId"
            :selectedCompany="companies.selectedValue"
            :selectedAlgorithm="algorithm.selectedValue"/>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>
<script>
import ScatterPlot from './ScatterPlot';
import LinePlot from './LinePlot';
export default {
  components: {ScatterPlot, LinePlot},
  data: () => ({
    categories: {
      values: ['All', 'tech', 'health', 'bank'],
      selectedValue: 'All'
    },
    companies: {
      values: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
      selectedValue: 1
    },
    algorithm: {
      values: ['none', 'random', 'regression'],
      selectedValue: 'none'
    },
    scatterPlotId: 0,
    linePlotId: 0,


    }),
    methods: {
    changeCategory() {
          this.scatterPlotId += 1
        },
    changeCompany() {
          this.linePlotId += 1
    },
    changeAlgorithm() {
          this.linePlotId += 1
    },
    changeCurrentlySelectedCompany(companyId) {
        this.companies.selectedValue = companyId
        this.changeCompany()
    },

  },
  watch: {
   selectedCategory: function () {
      this.ScatterPlotData.x = [];
      this.ScatterPlotData.y = [];
      this.fetchData();
   },

   selectedCompany() {
      this.LinePlotData.x = [];
      this.LinePlotData.y = [];
      this.fetchData();
   },

   selectedAlgorithm() {
      this.LinePlotData.x = [];
      this.LinePlotData.y = [];
      this.fetchData();
   },
   },
}
</script>

<style scoped>
.control-panel-font {
  font-family: "Open Sans", verdana, arial, sans-serif;
  align-items: center;
  font-size: 15px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  display: flex;
  font-weight: 500;
  height: 40px;
}
.sideBar {
  border-right: 1px solid rgba(0, 0, 0, 0.1);
  background: #fafafa;
  padding-left: 17px;
  height: calc(100vh - 50px);
}

.header {
  background: rgb(0, 162, 255);
  font-size: 30px;
  color: white;
  padding-left: 17px;
  
}
</style>