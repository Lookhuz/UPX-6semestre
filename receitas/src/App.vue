<template>
  <v-app>
    <Navbar />
    <v-main>
      <AboutUs />
      <MainProject />
      <ContactUs />
    </v-main>
    <Footer />
  </v-app>
</template>

<script>
import Navbar from './components/NavBar.vue';
import AboutUs from './components/AboutUs.vue';
import MainProject from './components/MainProject.vue';
import ContactUs from './components/ContactUs.vue';
import Footer from './components/Footer.vue';

export default {
  name: 'App',
  components: {
    Navbar,
    AboutUs,
    MainProject,
    ContactUs,
    Footer,
  },
};
</script>

<style>
body {
  background-color: #e8f5e9;
}
</style>
