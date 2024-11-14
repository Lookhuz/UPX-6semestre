<template>
  <v-app-bar app color="green darken-1" dark fixed>
    <v-toolbar-title>AgroCalendário</v-toolbar-title>
    <v-spacer></v-spacer>
    <v-btn text @click="$vuetify.goTo('#about-us')">About Us</v-btn>
    <v-btn text @click="$vuetify.goTo('#main-project')">Our Project</v-btn>
    <v-btn text @click="$vuetify.goTo('#contact-us')">Contact Us</v-btn>
  </v-app-bar>
</template>

<script>
export default {
  name: 'Navbar',
};
</script>

<style scoped>
</style>
