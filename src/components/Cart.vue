<template>
  <section class="shopping-cart spad">
    <div class="container">
      <div class="row">
        <div class="col-lg-8">
          <div class="row">
            <div class="col-lg-12">
              <div class="cart-table">
                <table>
                  <thead>
                    <tr>
                      <th>Image</th>
                      <th class="p-name text-center">Product Name</th>
                      <th>Price</th>
                      <th>Action</th>
                    </tr>
                  </thead>
                  <tbody v-if="productsCart.length > 0">

                    <tr v-for="data in productsCart" :key="data.id">
                      <td class="cart-pic first-row">
                        <img :src="data.photo" class="cart-page-cart"/>
                      </td>
                      <td class="cart-title first-row text-center">
                        <h5>{{ data.name }}</h5>
                      </td>
                      <td class="p-price first-row">Rp. IDR {{ data.price }}</td>
                      <td class="delete-item">
                        <a href="#"><i class="material-icons" @click="removeCart(data.id)"> close </i></a>
                      </td>
                    </tr>

                  </tbody>
                  <tbody>
                    <tr>
                      <td>tidak ada cart</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
            <div class="col-lg-8 text-left">
              <h4 class="mb-4">Informasi Pembeli:</h4>
              <div class="user-checkout">
                <form>
                  <div class="form-group">
                    <label for="namaLengkap">Nama lengkap</label>
                    <input
                      type="text"
                      class="form-control"
                      id="namaLengkap"
                      aria-describedby="namaHelp"
                      placeholder="Masukan Nama"
                      v-model="customerCheckout.name"
                    />
                  </div>
                  <div class="form-group">
                    <label for="namaLengkap">Email Address</label>
                    <input
                      type="email"
                      class="form-control"
                      id="emailAddress"
                      aria-describedby="emailHelp"
                      placeholder="Masukan Email"
                      v-model="customerCheckout.email"
                    />
                  </div>
                  <div class="form-group">
                    <label for="namaLengkap">No. HP</label>
                    <input
                      type="text"
                      class="form-control"
                      id="noHP"
                      aria-describedby="noHPHelp"
                      placeholder="Masukan No. HP"
                      v-model="customerCheckout.number"
                    />
                  </div>
                  <div class="form-group">
                    <label for="alamatLengkap">Alamat Lengkap</label>
                    <textarea
                      class="form-control"
                      id="alamatLengkap"
                      rows="3"
                      v-model="customerCheckout.address"
                    ></textarea>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>
        <div class="col-lg-4">
          <div class="row">
            <div class="col-lg-12">
              <div class="proceed-checkout text-left">
                <ul>
                  <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                  <li class="subtotal mt-3">Subtotal <span>Rp. IDR {{ total }}</span></li>
                  <li class="subtotal mt-3">Pajak <span>10% = Rp. IDR {{ (total* 0.10) }}</span></li>
                  <li class="subtotal mt-3">
                    Total Biaya <span>Rp. IDR {{ total + (total* 0.10) }}</span>
                  </li>
                  <li class="subtotal mt-3">
                    Bank Transfer <span>Mandiri</span>
                  </li>
                  <li class="subtotal mt-3">
                    No. Rekening <span>2208 1996 1403</span>
                  </li>
                  <li class="subtotal mt-3">
                    Nama Penerima <span>Shayna</span>
                  </li>
                </ul>
                <a class="proceed-btn text-white" @click="checkout()">I ALREADY PAID</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

export default {
  name: "Cart",
  data() {
    return {
      productsCart: [],
      cartsId: [],
      total: 0,
      customerCheckout: {
        "name" : "",
        "email" : "",
        "number" : "",
        "address" : "",
        "transaction_total": "",
        "transaction_status" : "PENDING",
        "transaction_details" : []
      }
    }
  },
  mounted() {
    this.productsCart = JSON.parse(localStorage.getItem('keranjangUser'))
    this.productsCart.forEach(data => {
      this.total += data.price
      this.cartsId.push(data.id)
    })
  },
  methods: {
      checkout() {
        this.customerCheckout.transaction_total = this.total
        this.customerCheckout.transaction_details = this.cartsId

        axios.post('http://127.0.0.1:8000/api/checkout', this.customerCheckout)
          .then(res => {
            console.log(res)
            this.$router.push({path: '/success'})
          })
          .catch(err => {
            console.log(err)
          })
      },
      removeCart(id) {
        const filtered = this.productsCart.filter(item => item.id != id)
        this.productsCart = filtered

        this.total = 0
        this.cartsId = []
        
        filtered.forEach(data => {
          this.total += data.price
          this.cartsId.push(data.id)
        })
        
        localStorage.setItem('keranjangUser', JSON.stringify(this.productsCart))
        this.$router.push({path: '/shopping-cart'})
      }
    },
};
</script>

<style scoped>
  .cart-page-cart {
    width: 100px;
    height: 150px;
    object-fit: cover;
  }
</style>