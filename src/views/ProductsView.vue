<template>
    <div class="container">
        <table class="table align-middle">
      <thead>
      <tr>
        <th>圖片</th>
        <th>商品名稱</th>
        <th>價格</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="product in productlist" :key="product.id">
      <td style="width: 200px">
      <div style="height: 100px; background-size: cover; background-position: center"
      :style="{ backgroundImage: 'url('+product.imageUrl+')' }">
      </div>
    </td>
        <td>
        </td>{{product.title}}<td>
          <!--<div class="h5">{{product.origin_price}}元</div>-->
          <del class="h6">原價{{product.origin_price}} 元</del>
          <div class="h5">現在只要 {{product.price}} 元</div>
        </td>
        <td>
          <div class="btn-group btn-group-sm">
            <button type="button" class="btn btn-outline-secondary" @click="selected(product)">
              <i class="fas fa-pulse"></i>
              查看更多
            </button>
            <button type="button" class="btn btn-outline-danger" @click="add2CartSingle(product.id)"
            :disabled="isLoading===product.id">
              <i class="fas fa-pulse" :class="{'fa-spinner':isLoading===product.id}"></i>
              加到購物車
            </button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
    </div>
    <ProductDetail ref="pmodal" :selected="selectedItem"></ProductDetail>
</template>

<script>
import ProductDetail from '@/components/ProductDetail.vue'
export default {
  data () {
    return {
      selectedItem: {},
      itemQty: {},
      productlist: [],
      isLoading: ''
    }
  },
  components: {
    ProductDetail
  },
  methods: {
    getProduct () {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`)
        .then((res) => {
          this.productlist = res.data.products
        })
        .catch((error) => { console.dir(error) })
    },

    selected (item) {
      this.selectedItem = item
      console.log(this.selectedItem)
      this.$refs.pmodal.modalShow()
    },

    add2CartSingle (idIn) {
      this.isLoading = idIn
      console.log(idIn)
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          const data = {
            product_id: idIn,
            qty: 1
          }
          this.$http.post(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`, { data })
            .then((res) => {
              console.log(res)
              // loadCart.emit('loadCart', this.selectedItem) // to cart.js for refreshing the cart
              this.isLoading = ''
            })
            .catch((error) => { console.dir(error) })
        }
        )
    }
  },

  mounted () {
    this.getProduct()
  }
}
</script>
