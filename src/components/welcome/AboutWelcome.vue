<template>
  <v-app>
    <v-content>
      <v-container class="mt-2">
        <v-row justify="center">
          <v-col cols="8">
            <v-card style="padding: 24px">
              <center>
                <v-avatar size="120px">
                  <v-img :src="avatarUrl" />
                </v-avatar>
                <div class="title mt-2">{{ username }}</div>
                <div class="subtitle-2">{{ name }}</div>
              </center>

              <br />

              <div class="headline">Repositories</div>

              <div v-for="item in repos" :key="item.id">
                <v-list-item two-line>
                  <v-list-item-content>
                    <v-list-item-title>{{ item.name }}</v-list-item-title>
                    <v-list-item-subtitle>{{ item.description }}</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>

                <v-divider />
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
      avatarUrl: "",
      username: "",
      name: "",
      repos: [],
      page: 1
    };
  },
  mounted() {
    fetch("https://api.github.com/users/erthru").then(result => {
      result.json().then(response => {
        this.avatarUrl = response.avatar_url;
        this.username = response.login;
        this.name = response.name;
      });
    });


    window.onscroll = () => {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight) {
        this.page += 1;

        this.fetchRepos();
      }
    };

    this.fetchRepos();
  },
  methods: {
    fetchRepos() {
      fetch("https://api.github.com/users/erthru/repos?page=" + this.page).then(
        result => {
          result.json().then(response => {
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