<template>
  <div class="product">
    <Header />
    <Breadcrumb title="Detail"/>
    <ProductDetail/>
    <RelatedProduct/>
    <Footer/>
  </div>
</template>

<script>
import Header from "../components/Header.vue";
import Footer from '../components/Footer.vue'
import Breadcrumb from '../components/Breadcrumb.vue'
import ProductDetail from '../components/ProductDetail.vue'
import RelatedProduct from '../components/RelatedProduct.vue'

export default {
  name: "Product",
  components: {
    Header,
    Breadcrumb,
    ProductDetail,
    RelatedProduct,
    Footer
  },
};
</script>