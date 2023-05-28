<template>
  <div
    class="product_display"
    :class="
      !isProductAvailable
        ? 'bg-gray'
        : product.data.category === 'men\'s clothing'
        ? 'bg-brightBlue'
        : 'bg-palePurple'
    "
  >
    <div v-if="loading">
      <Loader />
    </div>
    <div class="product_card">
      <!-- No Avaible -->
      <div
        class="product_cart-no-avaible"
        :class="isProductAvailable ? 'display-none' : 'product_cart-no-avaible'"
      >
        <p class="text-no">This product is unavailable to show</p>
        <button class="btn_no-avaible" @click="singleProduct()">Next product</button>
      </div>

      <!-- Avaible -->
      <div
        class="product_card-avaible"
        :class="!isProductAvailable ? 'display-none' : 'product_card-avaible'"
      >
        <!-- left -->
        <div class="product_card-left">
          <img :src="product?.data?.image" class="product_img" />
        </div>

        <!-- right -->
        <div class="product_card-right">
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
                    product?.data?.category === 'men\'s clothing' ? 'bg-darkBlue' : 'bg-darkPurple'
                  "
                ></div>
              </div>
            </div>
          </div>
          <hr />

          <p class="product_description">{{ product?.data?.description }}</p>

          <hr />
          <h2
            class="product_price"
            :class="
              product?.data?.category === 'men\'s clothing' ? 'color-darkBlue' : 'color-darkPurple'
            "
          >
            ${{ product?.data?.price }}
          </h2>

          <div class="btn_container">
            <button
              class="btn"
              :class="
                product?.data?.category === 'men\'s clothing'
                  ? 'font-white bg-darkBlue'
                  : 'font-white bg-darkPurple'
              "
            >
              Buy Now
            </button>
            <button
              @click="singleProduct()"
              class="btn btn-outline"
              :class="
                product?.data?.category === 'men\'s clothing'
                  ? 'border-darkBlue'
                  : 'border-darkPurple'
              "
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Loader from '../components/Loader.vue'

export default {
  name: 'ProductDisplayComponent',
  components: {
    Loader
  },

  data() {
    return {
      index: 0,
      product: {},
      maxRates: [1, 2, 3, 4, 5],
      loading: false,
      isProductAvailable: false
    }
  },

  methods: {
    async API() {
      try {
        const url = 'https://fakestoreapi.com/products/ ' + this.index
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
      this.loading = true

      if (this.index <= 20) {
        this.index++
      } else {
        this.index = 1
      }

      const data = await this.API()
      if (data.category === "men's clothing" || data.category === "women's clothing") {
        this.product = { data }
        this.isProductAvailable = true
      } else {
        this.isProductAvailable = false
      }

      this.loading = false
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
