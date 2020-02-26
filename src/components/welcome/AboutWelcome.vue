<template>
  <v-app>
    <v-content>
      <v-container class="mt-2">
        <v-row justify="center">
          <v-col cols="md-8 xs-1">
            <v-alert type="warning" dismissible>
              Api used:
              <a
                href="https://api.github.com/users/erthru"
                class="white--text"
                target="blank"
              >https://api.github.com/users/erthru</a>
            </v-alert>

            <v-card class="pa-4">
              <center>
                <v-progress-circular v-if="avatarIsLoading" color="primary" indeterminate />

                <v-avatar size="120px">
                  <v-img :src="avatarUrl" />
                </v-avatar>
                <div class="title mt-2">{{ username }}</div>
                <div class="subtitle-2">{{ name }}</div>
              </center>

              <br />

              <div class="headline">Repositories</div>

              <center>
                <v-progress-circular
                  v-if="repositoryIsLoading"
                  color="primary"
                  indeterminate
                  class="mt-4"
                />
              </center>

              <div v-for="item in repos" :key="item.id">
                <v-list-item two-line>
                  <v-list-item-content>
                    <v-list-item-title>{{ item.name }}</v-list-item-title>
                    <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>

                <v-divider />
              </div>

              <center>
                <v-progress-circular
                  v-if="repositoryNextIsLoading"
                  color="primary"
                  indeterminate
                  class="mt-4"
                />
              </center>
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
      avatarUrl: "",
      username: "",
      name: "",
      repos: [],
      page: 1,
      avatarIsLoading: false,
      repositoryIsLoading: false,
      repositoryNextIsLoading: false,
      reposTotal: 0
    };
  },
  mounted() {
    this.avatarIsLoading = true;

    fetch("https://api.github.com/users/erthru").then(result => {
      result.json().then(response => {
        this.avatarUrl = response.avatar_url;
        this.username = response.login;
        this.name = response.name;
        this.reposTotal = response.public_repos;

        this.avatarIsLoading = false;
      });
    });

    window.onscroll = () => {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        this.page += 1;

        if (this.repos.length < this.reposTotal) {
          this.fetchRepos();
        }
      }
    };

    this.fetchRepos();
  },
  methods: {
    fetchRepos() {
      if (this.page > 1) {
        this.repositoryNextIsLoading = true;
      } else {
        this.repositoryIsLoading = true;
      }

      fetch("https://api.github.com/users/erthru/repos?page=" + this.page).then(
        result => {
          result.json().then(response => {
            this.repositoryIsLoading = false;
            this.repositoryNextIsLoading = false;

            response.forEach(item => {
              this.repos.push(item);
            });
          });
        }
      );
    }
  }
};
</script>