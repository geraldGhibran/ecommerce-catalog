<template>
  <div class="product_display">
    <div class="product_card-avaible">
      <div class="product_card-right">
        <img :src="product?.data?.image" alt="" />
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
      product: {}
    }
  },

  methods: {
    async API() {
      try {
        const response = await fetch(`https://fakestoreapi.com/products/${this.index}`)
        console.log(response)
        if (!response.ok) {
          throw new Error('Request Failed')
        }
        const result = await response.json()
        this.product_display = result
      } catch (error) {
        this.error = error.message
      }
    }
  },

  mounted() {
    this.API()
  }
}
</script>

<style scoped>
@import url('../assets/style/page.css');
</style>
