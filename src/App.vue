<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
      class="app-bar"
    >
    <v-tool-bar-title>
      太陽光 発電量
    </v-tool-bar-title>
    <template v-slot:extension>
      <v-tabs
        v-model="tab"
        centered
      >
        <v-tab :href="tab-graph">グラフ</v-tab>
        <v-tab :href="tab-table">表</v-tab>
      </v-tabs>
    </template>
    </v-app-bar>

    <v-content>
      <LogTable :logs="logs"/>
    </v-content>
  </v-app>
</template>

<style>
.app-bar {
  text-align: center;
}
</style>


<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import LogTable from './components/LogTable.vue';

const csvServerUri = 'http://localhost:3000';

const csv2object = (csvText: string): Log[] => {
  const rowStrings = csvText.split('\n');
  const rowArrays = rowStrings.map((row) => row.split(','));
  const rowArraysValid = rowArrays.filter((row) => row.length > 12);

  return rowArraysValid.map((row) => ({
    date: new Date(row[0]),
    generatedPower: Number(row[1]),
    boughtPower: Number(row[2]),
    soldPower: Number(row[3]),
  }));
};

@Component({
  components: {
    LogTable,
  },
})
export default class App extends Vue {
  logs: Log[] = [];

  tab = 'tab-table';

  mounted() {
    this.axios.get(csvServerUri)
      .then((res) => {
        const csvText = res.data;
        this.logs = csv2object(csvText);
      })
      .catch((e) => { console.log(e); });
  }
}
</script>
