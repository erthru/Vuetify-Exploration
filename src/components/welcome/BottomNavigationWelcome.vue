<template>
  <v-app>
    <music-welcome v-if="botNavActive == 0" />
    <movies-welcome v-if="botNavActive == 1" />
    <about-welcome v-if="botNavActive == 2" />

    <v-bottom-navigation grow app color="pink" :value="botNavActive" v-on:change="botNavOnChange">
      <v-btn>
        <span>Music</span>
        <v-icon>mdi-music</v-icon>
      </v-btn>

      <v-btn>
        <span>Movies</span>
        <v-icon>mdi-movie</v-icon>
      </v-btn>

      <v-btn>
        <span>About</span>
        <v-icon>mdi-help</v-icon>
      </v-btn>
    </v-bottom-navigation>
  </v-app>
</template>

<script>
import MusicWelcome from "./MusicWelcome";
import MoviesWelcome from "./MoviesWelcome";
import AboutWelcome from "./AboutWelcome";

export default {
  components: {
    MusicWelcome,
    MoviesWelcome,
    AboutWelcome
  },
  data() {
    return {
      botNavActive: 0
    };
  },
  methods: {
    botNavOnChange(props) {
      this.botNavActive = props;
    }
  }
};
</script>