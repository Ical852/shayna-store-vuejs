<template>
  <div class="home">
    <Header />
    <Breadcrumb title="Shopping Cart"/>
    <Cart/>
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import Breadcrumb from '../components/Breadcrumb.vue'
import Cart from '../components/Cart.vue'

export default {
  name: "Home",
  components: {
    Header,
    Breadcrumb,
    Cart,
  },
};
</script>