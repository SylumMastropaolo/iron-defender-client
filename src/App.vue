<template>
  <v-app>
    <v-app-bar hide-on-scroll app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Iron Defender</span>
        <span class="font-weight-light"></span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text href="https://github.com/SylumMastropaolo" target="_blank">
        <span class="mr-2">My Github</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <HelloWorld />
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  components: {
    HelloWorld
  },
  data: () => ({
    //
  })
};
</script>
