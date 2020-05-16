<template>
  <v-content>
    <v-container>
      <v-card>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title> ジブリ映画</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item-group>
          <v-list-item v-for="film in filterDirector" :key="film.id">
            <v-list-item v-text="film.title"></v-list-item>
          </v-list-item>
        </v-list-item-group>
        <v-list-item-group>
          <v-list-item v-for="res in filterFactResponse" :key="res.id">
            <v-list-item v-text="res.text"> </v-list-item>
          </v-list-item>
        </v-list-item-group>
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
import { Component } from "nuxt-property-decorator";
import { Context } from "@nuxt/types";

export interface FactResponse {
  id: string;
  text: string;
  type: string;
  upvotes: number;
  user: {
    name: {
      first: string;
      last: string;
    };
  };
}
export interface GhibliFilm {
  id: string;
  title: string;
  description: string;
  director: string;
  producer: string;
  release_date: number;
  rt_score: number;
  url: string;
}
@Component({ computed: {} })
export default class index extends Vue {
  facts!: FactResponse[];
  films!: GhibliFilm[];
  item = 1;
  items = [
    { text: "Real-Time", icon: "mdi-clock" },
    { text: "Account", icon: "mdi-account" },
    { text: "Conversions", icon: "mdi-flag" }
  ];
  str = "hola";
  hours = new Date().getHours.toString;

  async asyncData(context: Context): Promise<object | void> {
    const response = await context.app.$axios.$get(
      "https://cat-fact.herokuapp.com/facts"
    );
    const films = await context.app.$axios.get(
      "https://ghibliapi.herokuapp.com/films"
    );
    const facts = response.all;

    return { facts, films: films.data };
  }

  getYear(): string {
    return "hola";
  }
  get filterFactResponse(): FactResponse[] {
    return this.facts.filter(f => {
      return f.type == "cat";
    }).filter( f => {
      return f.upvotes > 10; 
    });
  }
  get filterDirector(): GhibliFilm[]{
    return this.films.filter(f => {
      return f.director.includes('Miyazaki');
    });
  }
}
</script>

<style></style>
