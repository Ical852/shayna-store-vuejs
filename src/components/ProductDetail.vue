<template>
  <section class="product-shop spad page-details">
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <div class="row">
            <div class="col-lg-6">
              <div class="product-pic-zoom">
                <div v-if="gambar_default != {}">
                  <img class="product-big-img" :src="gambar_default" alt="" />
                </div>
              </div>
              <div class="product-thumbs" v-if="productDetail.galleries.length > 0">

                <Carousel class="product-thumbs-track ps-slider" :nav="false" :dots="false" :items="3">
                  <div v-for="data in productDetail.galleries" :key="data.id" 
                    :class="' pt ' + (data.img == gambar_default ? 'active' : '')" 
                    :data-imgbigurl="data.photo" @click="changeImage(data.photo)">
                    
                    <img :src="data.photo" alt="" class="other-img"/>
                  </div>
                </Carousel>

              </div>
            </div>
            <div class="col-lg-6">
              <div class="product-details text-left" v-if="productDetail != {}">
                <div class="pd-title">
                  <span >{{ productDetail.type }}</span>
                  <h3>{{ productDetail.name }}</h3>
                </div>
                <div class="pd-desc">
                  <p v-html="productDetail.description">
                  </p>
                  <h4>Rp. IDR {{ productDetail.price }}</h4>
                </div>
                <div class="quantity">
                  <!-- <router-link to="/shopping-cart"> -->
                    <a class="primary-btn pd-cart text-white" @click="saveToCart(productDetail)"> 
                      Add To Cart
                    </a>
                  <!-- </router-link> -->
                </div>
              </div>
              <div v-else>
                <p>Gettin Data</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
    name: 'ProductDetail',
    components: {
      Carousel
    },
    data() {
      return {
        detailId: this.$route.params.id,
        productDetail: {},
        gambar_default: "",
        keranjangUser: []
      }
    },
    methods: {
      changeImage(urlImage) {
        this.gambar_default = urlImage
      },
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
    mounted() {
      if (localStorage.getItem('keranjangUser')) {
        try {
          this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'))
        } catch (error) {
          localStorage.removeItem('keranjangUser')
        }
      }
      
      axios.get(`http://127.0.0.1:8000/api/products?id=${this.detailId}`)
          .then(res => {
            console.log(res.data.data)
            this.productDetail = res.data.data
            this.gambar_default = res.data.data.galleries[0].photo
          })
          .catch(err => {
            console.log(err)
          })
    },
}
</script>

<style scoped>
  .pt {
    margin-right: 10px;
  }
  .other-img {
    height: 200px;
    max-width: 180px;
    object-fit: cover;
  }
</style>