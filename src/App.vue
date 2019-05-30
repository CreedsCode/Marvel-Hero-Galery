<template>
  <v-app>
    <v-toolbar class="red" app>
      <v-toolbar-title class="white--text">Marvel Heroes</v-toolbar-title>
    </v-toolbar>
    <v-content>
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-content>
    <v-footer height="auto" class="white--text red" app>
      <v-flex text-xs-center white--text xs12>
        Data provided by Marvel. <strong>MARVEL</strong> — &copy; 2019
      </v-flex>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      //
    };
  }
};
</script>
