<template>
  <v-app>
    <v-content>
      <v-container class="mt-2">
        <v-row justify="center">
          <v-col cols="md-8 xs-1">
            <v-alert type="warning" dismissible>
              Api used:
              <a
                href="http://www.omdbapi.com/"
                class="white--text"
                target="blank"
              >http://www.omdbapi.com/</a>
            </v-alert>

            <v-card class="pa-4">
              <div v-if="!onSearch">
                <center>
                  <v-col cols="md-6 xs-1">
                    <div class="display-1">Search Movies</div>

                    <v-form lazy-validation ref="form" v-on:submit.prevent="search" class="mt-3">
                      <v-text-field
                        v-model="searchVal"
                        placeholder="Ex: Naruto Shippuden"
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
                      >Search Movies</v-btn>
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

                <v-row class="mt-2">
                  <v-col cols="md-4 xs-1" v-for="item in searchResults" :key="item.imdbID">
                    <v-card>
                      <v-img :src="item.Poster" height="200px" />

                      <v-card-title>
                        <span class="single-line subtitle-2">{{ item.Title }}</span>
                      </v-card-title>
                      <v-card-subtitle>{{ item.Year }}</v-card-subtitle>
                    </v-card>
                  </v-col>
                </v-row>

                <center>
                  <v-progress-circular
                    v-if="isLoadingNext"
                    color="primary"
                    indeterminate
                    class="mt-4"
                  />
                </center>
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
      btnIsLoading: false,
      page: 1,
      searchResults: [],
      searchTotal: 0,
      isSearchDisabled: false,
      errorMsg: "",
      isLoadingNext: false,

      // form rules
      searchRules: [
        v => !!v || "Search keyword is required",
        v => (v && v.length >= 3) || "Keyword must be more than 2 characters"
      ]
    };
  },
  mounted() {
    window.onscroll = () => {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        if (this.searchResults.length < this.searchTotal) {
          this.next();
        }
      }
    };
  },
  methods: {
    search() {
      if (this.$refs.form.validate()) {
        this.errorMsg = "";
        this.btnIsLoading = true;
        this.isSearchDisabled = true;
        this.searchResults = [];
        this.page = 1;

        fetch(
          "http://www.omdbapi.com/?apikey=bdeffd3f&type=movie&s=" +
            this.searchVal +
            "&page=" +
            this.page
        ).then(result => {
          result.json().then(response => {
            if (response.Search == null) {
              this.errorMsg = "Movie not found";
              this.isSearchDisabled = false;
              this.btnIsLoading = false;
            } else {
              this.searchResults = response.Search;
              this.btnIsLoading = false;
              this.isSearchDisabled = false;
              this.onSearch = true;
              this.searchTotal = response.totalResults;
            }
          });
        });
      }
    },
    next() {
      this.page += 1;
      this.isLoadingNext = true;

      fetch(
        "http://www.omdbapi.com/?apikey=bdeffd3f&type=movie&s=" +
          this.searchVal +
          "&page=" +
          this.page
      ).then(result => {
        result.json().then(response => {
          response.Search.forEach(item => {
            this.searchResults.push(item);
          });

          this.isLoadingNext = false;
        });
      });
    }
  }
};
</script>