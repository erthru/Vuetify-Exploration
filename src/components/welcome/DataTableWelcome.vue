<template>
  <v-app>
    <v-content>
      <v-container class="mt-2">
        <v-row justify="center">
          <v-col cols="8">
            <v-alert type="warning" dismissible>
              Api used:
              <a
                href="https://reqres.in/api/users"
                class="white--text"
                target="blank"
              >https://reqres.in/api/users</a>
              <br />Data Table using server side rendering
              <br />Reqres.in not provide search, so search not working
            </v-alert>

            <v-card>
              <v-card-title>
                Users
                <v-spacer />

                <v-text-field
                  append-icon="mdi-magnify"
                  label="Search"
                  single-line
                  hide-details
                  v-on:change="tblUserOnSearch"
                ></v-text-field>
              </v-card-title>

              <v-data-table
                :headers="tblUserHeaders"
                :items="tblUserItems"
                :loading="tblUserIsLoading"
                :options="tblUserOptions"
                :server-items-length="tblUserItemLength"
                v-on:pagination="tblUserOnPaginate"
              >
                <template v-slot:item.avatar="{ item }">
                  <v-avatar size="40px">
                    <v-img :src="item.avatar" />
                  </v-avatar>
                </template>
              </v-data-table>
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
      // table user
      tblUserHeaders: [
        { text: "Email", value: "email", sortable: false },
        { text: "First Name", value: "first_name", sortable: false },
        { text: "Last Name", value: "last_name", sortable: false },
        { text: "Avatar", value: "avatar", sortable: false }
      ],
      tblUserItems: [],
      tblUserIsLoading: true,
      tblUserOptions: {
        page: 1,
        itemsPerPage: 5
      },
      tblUserItemLength: 0
    };
  },
  methods: {
    tblUserOnPaginate(props) {
      this.tblUserIsLoading = true;

      fetch(
        "https://reqres.in/api/users?page=" +
          props.page +
          "&per_page=" +
          props.itemsPerPage +
          "&delay=3"
      ).then(result => {
        result.json().then(response => {
          this.tblUserItemLength = response.total;
          this.tblUserItems = [];

          response.data.forEach(item => {
            this.tblUserItems.push({
              email: item.email,
              first_name: item.first_name,
              last_name: item.last_name,
              avatar: item.avatar
            });
          });

          this.tblUserIsLoading = false;
        });
      });
    },
    tblUserOnSearch(props) {
      if (props == "") {
        this.tblUserOnPaginate(this.tblUserOptions);
      } else {
        this.tblUserIsLoading = true;
      }
    }
  }
};
</script>