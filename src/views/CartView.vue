<template>
    <div class="container">
        <div class="text-end">
    <button class="btn btn-outline-danger" type="button" @click="delCart"
    :disabled="cart.length===0">清空購物車</button>
  </div>
  <table class="table align-middle">
    <thead>
      <tr>
        <th></th>
        <th>品名</th>
        <th style="width: 150px">數量/單位</th>
        <th>單價</th>
      </tr>
    </thead>
    <tbody>
      <template v-if="cart">
        <tr v-for="item in cart" :key=item.product.id>
          <td>
            <button type="button" class="btn btn-outline-danger btn-sm" @click="delItem(item.id)">
              <i class="fas fa-pulse" :class="{'fa-spinner':isDeleting===item.id}"></i>
              x
            </button>
          </td>
          <td>
            {{item.product.title}}
          </td>
          <td>
            <div class="input-group input-group-sm">
              <div class="input-group mb-3">
                <select id="" class="form select" v-model="item.qty" @change="updateCart(item)" :disabled="isLoading===item.id">
                  <option :value="num" v-for="num in 20" :key="num">
                    {{num}}
                  </option>
                </select>
                <span class="input-group-text" id="basic-addon2">{{ item.product.unit }}</span>
              </div>
            </div>
          </td>
          <td class="">
            <small class="text-success">折扣價：</small>
            {{ item.product.price }}
          </td>
        </tr>
      </template>
    </tbody>
    <tfoot>
      <tr>
        <td colspan="3" class="text-end">總計</td>
        <td class="text-end">{{}}</td>
      </tr>
      <!--<tr>
        <td colspan="3" class="text-end text-success">折扣價</td>
        <td class="text-end text-success">{{}}</td>
      </tr> -->
    </tfoot>
  </table>
    </div>
  <FormBody v-on:ref-cart='getCart' :cart="cart"></FormBody>
</template>

<script>
import FormBody from '@/components/FormBody.vue'
export default {
  data () {
    return {
      cart: {},
      isLoading: '',
      isDeleting: ''
    }
  },
  components: {
    FormBody
  },

  methods: {
    getCart () {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          this.cart = res.data.data.carts
          console.log('Cart Loaded')
        })
        .catch((error) => { console.dir(error) })
    },

    delCart () {
      this.$http.delete(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/carts`)
        .then((res) => {
          this.getCart()
        })
        .catch((error) => { console.dir(error) })
    },

    delItem (id) {
      this.isDeleting = id
      console.log(id)
      this.$http.delete(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${id}`)
        .then((res) => {
          this.getCart()
          this.isDeleting = ''
        })
        .catch((error) => { console.dir(error) })
    },

    updateCart (item) {
      this.isLoading = item.id
      const data = {
        product_id: item.id,
        qty: item.qty
      }

      this.$http.put(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${item.id}`, { data })
        .then((res) => {
          console.log(res)
          this.isLoading = ''
          this.getCart()
        })
        .catch((error) => { console.dir(error) })
    }
  },

  mounted () {
    this.getCart()
  }
}
</script>
