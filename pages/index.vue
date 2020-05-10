<template>
  <v-content>
    <v-container>
      <v-card>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title> 宅配お弁当 {{ str }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item>
          <v-list-item-content>
            {{ filterResponse }}
          </v-list-item-content>
        </v-list-item>
      </v-card>
      <v-list>
        <v-subheader>REPORTS</v-subheader>
        <v-list-item-group color="primary">
          <v-list-item v-for="(item, i) in items" :key="i">
            <v-list-item-icon>
              <v-icon v-text="item.icon"></v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title v-text="item.text"></v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-container>
  </v-content>
</template>

<script lang="ts">
import Vue from "vue";

interface FactResponse {
  id: string;
  text: string;
  type: string;
  upvotes: number;
  user: {
    name: {
      first: string;
      last: string;
    }
  },
}
export default Vue.extend({
  async asyncData({ app }){
    const response = await app.$axios.$get('https://cat-fact.herokuapp.com/facts');
    const arrays: FactResponse[] = response.all;
    return {response, arrays}

  },
  data: () =>( {
    response: '',
    arrays: [],
    item: 1,
    items: [
      { text: "Real-Time", icon: "mdi-clock" },
      { text: "Account", icon: "mdi-account" },
      { text: "Conversions", icon: "mdi-flag" }
    ],
    str: "hola",
    hours: new Date().getHours.toString
  }),
  computed: {
    filterResponse: function(){
      return JSON.stringify(this.arrays.slice(0, 3));
    }
  },
  methods: {
    getYear(): string {
      return "hola";
    }
  }
});
</script>

<style></style>
