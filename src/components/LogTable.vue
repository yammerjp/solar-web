<template>
  <v-container>
    <v-data-table
      :headers="headers"
      :items="body"
      :items-per-page="50"
    />
  </v-container>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';

const formatDate = (date: Date, formatArg: string) => {
  let format = formatArg;
  format = format.replace(/yyyy/g, String(date.getFullYear()));
  format = format.replace(/MM/g, (`0${date.getMonth() + 1}`).slice(-2));
  format = format.replace(/dd/g, (`0${date.getDate()}`).slice(-2));
  format = format.replace(/HH/g, (`0${date.getHours()}`).slice(-2));
  format = format.replace(/mm/g, (`0${date.getMinutes()}`).slice(-2));
  format = format.replace(/ss/g, (`0${date.getSeconds()}`).slice(-2));
  format = format.replace(/SSS/g, (`00${date.getMilliseconds()}`).slice(-3));
  return format;
};

@Component
export default class HelloWorld extends Vue {
  @Prop({ type: Array, required: true })
  logs: Log[];

  headers = [
    { text: '日時', value: 'date' },
    { text: '発電力(kW)', value: 'generatedPower' },
    { text: '買電力(kW)', value: 'boughtPower' },
    { text: '売電力(kW)', value: 'soldPower' },
  ]

  // eslint-disable-next-line
  private get body(): any[] {
    return this.logs.map(({
      date, generatedPower, boughtPower, soldPower,
    }) => ({
      date: formatDate(date, 'yyyy/MM/dd HH:mm'),
      generatedPower,
      boughtPower,
      soldPower,
    }));
  }
}

</script>
