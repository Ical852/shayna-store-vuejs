<template>
  <header class="header-section">
    <div class="header-top">
      <div class="container">
        <div class="ht-left">
          <div class="mail-service">
            <i class="fa fa-envelope"></i> hello.shayna@gmail.com
          </div>
          <div class="phone-service">
            <i class="fa fa-phone"></i> +628 22081996
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="inner-header">
        <div class="row">
          <div class="col-lg-2 col-md-2">
            <div class="logo">
              <router-link to="/">
                <img src="img/logo_website_shayna.png" alt="" />
              </router-link>
            </div>
          </div>
          <div class="col-lg-7 col-md-7"></div>
          <div class="col-lg-3 text-right col-md-3">
            <ul class="nav-right">
              <li class="cart-icon">
                Keranjang Belanja &nbsp;
                
                <a href="#" v-if="cart.length > 0">
                  <i class="icon_bag_alt"></i>
                  <span>{{ cart.length }}</span>
                </a>
                <a href="" v-else>
                  <i class="icon_bag_alt"></i>
                </a>

                <div class="cart-hover" v-if="cart.length > 0">
                  <div class="select-items">
                    <table>
                      <tbody>

                        <tr v-for="data in cart" :key="data.id">
                          <td class="si-pic">
                            <img :src="data.photo" alt="" class="cart-img-header"/>
                          </td>
                          <td class="si-text">
                            <div class="product-selected">
                              <p>Rp. IDR {{ data.price }}</p>
                              <h6>{{ data.name }}</h6>
                            </div>
                          </td>
                          <td class="si-close" @click="removeCart(data.id)">
                            <i class="ti-close"></i>
                          </td>
                        </tr>

                      </tbody>
                    </table>
                  </div>
                  <div class="select-total">
                    <span>total:</span>
                    <h5>Rp. IDR {{ total }}</h5>
                  </div>
                  <div class="select-button">
                    <router-link to="/shopping-cart" style="width: 100%">
                      <a href="#" class="primary-btn view-card">VIEW CARD</a>
                    </router-link>
                    <a href="#" class="primary-btn checkout-btn" @click="checkout()">CHECK OUT</a>
                  </div>
                </div>

                <div class="cart-hover" v-else>
                    <h5>Belum Ada Barang di Keranjang</h5>
                </div>

              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
    name: 'Header',
    data() {
      return {
        cart: [],
        total: 0,
        productsId: []
      }
    },
    mounted() {
      this.cart = JSON.parse(localStorage.getItem('keranjangUser'))
      this.cart.forEach(data => {
        this.total += data.price
        this.productsId.push(data.id)
      })
    },
    methods: {
      checkout() {
        if (confirm('checkout ? ')) {
          console.log('success')
          this.$router.push({path: '/success'})
        }
      },
      removeCart(id) {
        const filtered = this.cart.filter(item => item.id != id)
        this.cart = filtered

        this.total = 0
        this.productsId = []
        
        filtered.forEach(data => {
          this.total += data.price
          this.productsId.push(data.id)
        })
        
        localStorage.setItem('keranjangUser', JSON.stringify(this.cart))
      }
    },
}
</script>

<style scoped>
  .cart-img-header {
    width: 50px;
    height: 50px;
    object-fit: cover;
  }
</style>