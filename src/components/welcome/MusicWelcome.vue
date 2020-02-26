<template>
  <v-app>
    <v-content>
      <v-container class="mt-2">
        <v-row justify="center">
          <v-col cols="md-8 xs-1">
            <v-alert type="warning" dismissible>
              Api used:
              <a
                href="http://api.napster.com/v2.2/"
                class="white--text"
                target="blank"
              >http://api.napster.com/v2.2/</a>
            </v-alert>

            <v-card class="pa-4">
              <div v-if="!onSearch">
                <center>
                  <v-col cols="md-6 xs-1">
                    <div class="display-1">Search Music</div>

                    <v-form lazy-validation ref="form" v-on:submit.prevent="search" class="mt-3">
                      <v-text-field
                        v-model="searchVal"
                        placeholder="Ex: Ninelie"
                        single-line
                        solo
                        :rules="searchRules"
                        required
                        :disabled="isSearchDisabled"
                      />

                      <v-btn
                        style="margin-top: -10px"
                        color="primary"
                        :loading="btnIsLoading"
                        v-on:click="search"
                      >Search Music</v-btn>
                    </v-form>

                    <v-alert
                      style="margin-top: 20px"
                      type="error"
                      dismissible
                      v-if="errorMsg"
                    >{{ errorMsg }}</v-alert>
                  </v-col>
                </center>
              </div>

              <div v-if="onSearch">
                <div class="subtitle-1">
                  <v-icon class="mb-1" v-on:click="onSearch = false">mdi-arrow-left</v-icon>
                  <span class="ml-2">Showing result for:</span>
                  <span class="primary--text">{{ searchVal }}</span>
                </div>

                <v-list-item three-line v-for="item in searchResults" :key="item.id">
                  <v-list-item-avatar>
                    <v-icon
                      size="37px"
                      v-if="musicPlayingId != item.id"
                      v-on:click="musicPlayingId = item.id; musicOnPlaying = item.previewURL; play()"
                    >mdi-play-circle</v-icon>
                    <v-icon
                      size="37px"
                      v-if="musicPlayingId == item.id"
                      v-on:click="musicPlayingId = ''; stop()"
                    >mdi-pause-circle</v-icon>
                  </v-list-item-avatar>

                  <v-list-item-content>
                    <v-list-item-title>{{ item.name }}</v-list-item-title>
                    <v-list-item-subtitle>{{ item.artistName }}</v-list-item-subtitle>
                    <v-divider class="mt-2" />
                  </v-list-item-content>
                </v-list-item>

              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      onSearch: false,
      searchVal: "",
      errorMsg: "",
      btnIsLoading: false,
      page: 1,
      searchResults: [],
      isSearchDisabled: false,
      musicPlayingId: "",
      musicOnPlaying: "",
      audio: null,

      // form rules
      searchRules: [
        v => !!v || "Search keyword is required",
        v => (v && v.length >= 3) || "Keyword must be more than 2 characters"
      ]
    };
  },
  methods: {
    search() {
      this.btnIsLoading = true;
      this.searchResults = [];
      this.isSearchDisabled = true;

      if (this.$refs.form.validate()) {
        fetch(
          "http://api.napster.com/v2.2/search?apikey=NGRkYmUyYzktMzNiMC00MTAwLTk2MTUtZDQzZDRiN2VmMjdm&query=" +
            this.searchVal +
            "&type=track"
        ).then(result => {
          result.json().then(response => {
            this.searchResults = response.search.data.tracks;
            this.btnIsLoading = false;
            this.isSearchDisabled = false;
            this.onSearch = true;
          });
        });
      }
    },
    play() {
      if (this.audio != null) {
        this.stop();
      }

      this.audio = new Audio(this.musicOnPlaying);
      this.audio.play();
    },
    stop() {
      if (this.audio != null) {
        this.audio.pause();
        this.audio.currentTime = 0;
      }
    }
  }
};
</script>