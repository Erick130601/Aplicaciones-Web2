<template>
  <label>Lis of users</label>

  <pv-input-text v-model="start" type="number">Start</pv-input-text>
  <pv-input-text v-model="end" type="number">End</pv-input-text>
  <pv-button @click="refresh()">Refresh</pv-button>
<br><br>
  <pv-database :value="users" tableStyle="min-width: 20rem">
    <pv-column v-for="col in users" :key="col.field" :field="col.field" :header="col.header"></pv-column>
  </pv-database>

<!--  <div v-for="user in users">-->
<!--    <div>{{ user.id + " - " + user.name }}</div>-->
<!--  </div>-->
</template>



<script>
import UseApiService from "../services/use-api.service";


export default {
  name: "user-list.component",

  data() {
    return {
      users: [],
      columns:null,
      start: 0,
      end: 10,
    };
  },
  created() {
    this.users = [
      { field: 'id', header: 'id' },
      { field: 'name', header: 'Name' },

    ];
  },
  async mounted() {
    new UseApiService()
      .getAll()
      .then((response) => (this.users = response.data));
  },
  methods: {
    refresh() {
      new UseApiService()
        .getByRange(this.start, this.end)
        .then((response) => (this.users = response.data));
    },
  },
};
</script>

<style scoped></style>
