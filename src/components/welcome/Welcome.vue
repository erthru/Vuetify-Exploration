<template>
  <v-app>
    <v-app-bar color="pink" dark app>
      <v-app-bar-nav-icon v-on:click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>
        <router-link to="/" class="white--text" style="text-decoration: none">VuetifyExploration</router-link>
      </v-toolbar-title>

      <v-spacer />

      <v-menu left bottom>
        <template v-slot:activator="{ on }">
          <v-btn icon v-on="on">
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>

        <v-list>
          <v-list-item>
            <v-list-item-title>Setting</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>

      <template v-slot:extension>
        <v-tabs align-with-title v-model="tab">
          <v-tab>
            <v-icon>mdi-table</v-icon>
            <span class="ml-2">Data Table</span>
          </v-tab>
          <v-tab>
            <v-icon>mdi-navigation</v-icon>
            <span class="ml-2">Bottom Navigation</span>
          </v-tab>
        </v-tabs>
      </template>
    </v-app-bar>

    <v-tabs-items v-model="tab">
      <v-tab-item>
        <data-table-welcome />
      </v-tab-item>

      <v-tab-item>
        <bottom-navigation-welcome />
      </v-tab-item>
    </v-tabs-items>

    <v-navigation-drawer v-model="drawer" absolute temporary>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title>
            <router-link
              to="/"
              class="headline secondary--text"
              style="text-decoration: none"
            >VuetifyExploration</router-link>
          </v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-divider />

      <v-list>
        <v-list-item link v-on:click="tab = 0; drawer = !drawer">
          <v-list-item-icon>
            <v-icon>mdi-table</v-icon>
          </v-list-item-icon>
          <v-list-item-content style="margin-left:-20px">
            <v-list-item-title>Data Table</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item link v-on:click="tab = 1; drawer = !drawer">
          <v-list-item-icon>
            <v-icon>mdi-navigation</v-icon>
          </v-list-item-icon>
          <v-list-item-content style="margin-left:-20px">
            <v-list-item-title>Bottom Navigation</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-app>
</template>

<script>
import DataTableWelcome from "./DataTableWelcome";
import BottomNavigationWelcome from "./BottomNavigationWelcome";

export default {
  components: {
    DataTableWelcome,
    BottomNavigationWelcome
  },
  data() {
    return {
      tab: 0,
      drawer: false
    };
  }
};
</script>