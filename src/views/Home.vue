<template>
  <div class="home">
    <Header />
    <Hero />
    <WomenBanner/>
    <Instagram/>
    <PartnerLogo/>
    <Footer/>
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import Hero from '../components/Hero.vue';
import WomenBanner from '../components/WomenBanner.vue'
import Instagram from '../components/Instagram.vue'
import PartnerLogo from '../components/PartnerLogo.vue'
import Footer from '../components/Footer.vue'

export default {
  name: "Home",
  components: {
    Header,
    Hero,
    WomenBanner,
    Instagram,
    PartnerLogo,
    Footer
  },
};
</script>