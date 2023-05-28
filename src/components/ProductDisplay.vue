<template>
  <div class="product_display">
    <div class="product_card">
      <div
        class="product_card-avaible"
        :class="isProductAvailable ? 'display-none' : 'product_card-avaible'"
      >
        <div class="product_card-right">
          <img :src="product?.data?.image" class="product_img" />
        </div>

        <div class="product_card-left">
          <h1>{{ product?.data?.title }}</h1>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductDisplayComponent',

  data() {
    return {
      index: 0,
      product: {},
      isProductAvailable: false
    }
  },

  methods: {
    async API() {
      try {
        const url = `https://fakestoreapi.com/products/${this.index}`
        const response = await fetch(url)
        if (!response.ok) {
          throw new Error('Request Failed')
        }
        const result = await response.json()
        return result
      } catch (error) {
        this.error = error.message
      }
    },

    async singleProduct() {
      if (this.index <= 20) {
        this.index++
      } else {
        this.index = 1
      }

      const data = await this.API()
      console.log(data)
      if (data.category === "men's clothing" || data.category === "women's clothing") {
        this.product = { data }
        this.isProductAvailable = true
      } else {
        this.isProductAvailable = false
      }
    }
  },

  mounted() {
    this.singleProduct()
  }
}
</script>

<style scoped>
@import url('../assets/style/page.css');
</style>
