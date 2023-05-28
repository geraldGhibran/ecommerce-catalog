<template>
  <div class="product_display">
    <div class="product_card">
      <div
        class="product_card-avaible"
        :class="isProductAvailable ? 'display-none' : 'product_card-avaible'"
      >
        <!-- Right -->
        <div class="product_card-right">
          <img :src="product?.data?.image" class="product_img" />
        </div>

        <!-- Left -->
        <div class="product_card-left">
          <h1
            class="product_title"
            :class="
              product?.data?.category === 'men\'s clothing' ? 'color-darkBlue' : 'color-darkPurple'
            "
          >
            {{ product?.data?.title }}
          </h1>

          <div class="product_information">
            <p class="product_category">{{ product?.data?.category }}</p>
            <div class="product_rating">
              <p class="product_rating_rate">{{ product?.data?.rating?.rate }} / 5</p>
              <div class="product_rating_circle">
                <div
                  v-for="maxRate in maxRates"
                  :key="maxRate"
                  class="circle"
                  :class="
                    product?.data?.category === 'men\'s clothing'
                      ? 'color-darkBlue'
                      : 'color-darkPurple'
                  "
                ></div>
              </div>
            </div>
          </div>
          <hr />

          <p>{{ product?.data?.description }}</p>
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
      maxRates: [1, 2, 3, 4, 5],
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

<style>
@import url('../assets/style/page.css');
</style>
