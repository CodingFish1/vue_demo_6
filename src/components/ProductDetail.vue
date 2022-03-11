<template>
<div class="modal fade" id="productModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true" ref="modaldom">
    <div class="modal-dialog modal-xl" role="document" >
      <div class="modal-content border-0">
        <div class="modal-header bg-dark text-white">
          <h5 class="modal-title" id="exampleModalLabel">
            <span>{{selected.title}}</span>
          </h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-sm-6">
              <img class="img-fluid" v-bind:src="selected.imageUrl" alt="">
            </div>
            <div class="col-sm-6">
              <span class="badge bg-primary rounded-pill">{{ }}</span>
              <p>商品描述：{{selected.description}}</p>
              <p>商品內容：{{selected.content}}</p>
              <div class="h5">{{selected.price}} 元</div>
              <del class="h6">原價 {{selected.origin_price}} 元</del>
              <div class="h5">現在只要 {{selected.price}} 元</div>
              <div>
                <div class="input-group">
                  <input type="number" class="form-control" min="1" v-model.number="itemCounter"
                  ref="blocker" @change="filter">
                  <button type="button" class="btn btn-primary" @click="add2Cart" :disabled="isDisable">加入購物車</button>
                </div>
                {{warning}}
              </div>
            </div>
            <!-- col-sm-6 end -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Modal from 'bootstrap/js/dist/modal'
export default {
  data () {
    return {
      modalOn: '',
      itemCounter: '',

      isDisable: false,
      warning: ''
    }
  },

  props: ['selected'],

  methods: {
    add2Cart () {
      const data = {
        product_id: this.selected.id,
        qty: this.itemCounter
      }

      this.$http.post(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`, { data })
        .then((res) => {
          console.log(res)
          this.modalOn.hide()
        })
        .catch((error) => { console.dir(error) })
    },

    filter () {
      const inputNum = Number(this.$refs.blocker.value)
      if (inputNum <= 0 || inputNum === 'e' || (Math.ceil(inputNum) !== Math.floor(inputNum))) {
        this.warning = '請輸入合法訂單數量'
        this.isDisable = true
      } else {
        this.warning = ''
        this.isDisable = false
      }
    },

    modalShow () {
      this.modalOn.show()
    }
  },

  mounted () {
    this.modalOn = new Modal(this.$refs.modaldom)
  },

  unmounted () {
  },

  created () {
  }
}
</script>
