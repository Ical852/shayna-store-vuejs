<template>
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">

        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <Carousel class="product-slider" :nav="false" :dots="false" :loop="true" :autoplay="true">
            
            <div class="product-item" v-for="data in products" v-bind:key="data.id">
              <div class="pi-pic">
                <img :src="data.galleries[0].photo" alt="" class="product-img"/>
                <ul>
                  <li class="w-icon active">
                    <a href="#" @click="saveToCart(data)"><i class="icon_bag_alt"></i></a>
                  </li>
                  <li class="quick-view">
                    <router-link :to="'/product/'+ data.id">
                      + Quick View
                    </router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ data.type }}</div>
                <router-link :to="'/product/'+ data.id">
                  <h5>{{ data.name }}</h5>
                </router-link>
                <div class="product-price">
                  Rp. IDR {{ data.price + (data.price * 0.5) }}
                  <br>
                  <span>Rp. IDR {{ data.price }} </span>
                </div>
              </div>
            </div>
            
          </Carousel>
        </div>
        <div class="col-lg-12" v-else>
          <p>On Processing...</p>
        </div>

      </div>
    </div>
  </section>
</template>

<script>
import Carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
  name: "WomenBanner",
  components: {
    Carousel
  },
  data() {
    return {
      products: [],
      keranjangUser: []
    }
  },
  mounted() {
    if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'))
      } catch (error) {
        localStorage.removeItem('keranjangUser')
      }
    }

    axios.get("http://127.0.0.1:8000/api/products")
      .then(res => {
        this.products = res.data.data.data
      })
      .catch(err => {
        console.log(err)
      })
  },
  methods: {
      saveToCart(productData) {
        const data = {
          id: productData.id,
          name: productData.name,
          price: productData.price,
          photo: productData.galleries[0].photo
        }

        this.keranjangUser.push(data)
        const parsed = JSON.stringify(this.keranjangUser)
        localStorage.setItem('keranjangUser', parsed)

        this.$router.push({path: '/shopping-cart'})
      }
    },
};
</script>

<style scoped>
    .product-item {
        margin-right: 25px;
        max-width: 500px;
    }

    .product-img {
      height: 420px;
      object-fit: cover;
    }
</style>