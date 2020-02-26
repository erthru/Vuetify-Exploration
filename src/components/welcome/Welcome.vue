<template>
  <v-app>
    <v-app-bar color="pink lighten-1" dark app>
      <v-toolbar-title>
        <router-link to="/" class="white--text" style="text-decoration: none">VuetifyExploration</router-link>
      </v-toolbar-title>

      <v-spacer />

      <template v-slot:extension>
        <v-tabs align-with-title v-model="tab">
          <v-tab>
            <v-icon>mdi-table</v-icon>
            <span class="ml-2">Table</span>
          </v-tab>
          <v-tab>
            <v-icon>mdi-music</v-icon>
            <span class="ml-2">Music</span>
          </v-tab>
          <v-tab>
            <v-icon>mdi-video</v-icon>
            <span class="ml-2">Movies</span>
          </v-tab>
          <v-tab>
            <v-icon>mdi-help</v-icon>
            <span class="ml-2">About</span>
          </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>

    <v-tabs-items v-model="tab">
      <v-tab-item>
        <table-welcome />
      </v-tab-item>

      <v-tab-item>
        <music-welcome />
      </v-tab-item>

      <v-tab-item>
        <movies-welcome />
      </v-tab-item>

      <v-tab-item>
        <about-welcome />
      </v-tab-item>
    </v-tabs-items>

  </v-app>
</template>

<script>
import TableWelcome from "./TableWelcome";
import MusicWelcome from "./MusicWelcome";
import MoviesWelcome from "./MoviesWelcome";
import AboutWelcome from "./AboutWelcome";

export default {
  components: {
    TableWelcome,
    MusicWelcome,
    MoviesWelcome,
    AboutWelcome
  },
  data() {
    return {
      tab: 0
    };
  }
};
</script>