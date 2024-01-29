<template>
  <div
    class="product-display"
    :class="
      !productAvailable
        ? 'bg-gray'
        : products.data.category === 'men\'s clothing'
        ? 'bg-brightBlue'
        : 'bg-palePurple'
    "
  >
    <div v-if="loading">
      <SkeletonDisplay />
    </div>
    <div class="product-card">
      <!-- No Avaible -->
      <div
        class="product-cart-no-available"
        :class="productAvailable ? 'display-none' : 'product-cart-no-avaible'"
      >
        <p class="text-no">This product is unavailable to show</p>
        <button class="btn_no-available" @click="nextProduct()">
          Next product
        </button>
      </div>

      <!-- Available -->
      <div
        class="product-card-available"
        :class="!productAvailable ? 'display-none' : 'product-card-avaible'"
      >
        <!-- left -->
        <div class="product-card-left">
          <img :src="products?.data?.image" class="product-img" />
        </div>

        <!-- right -->
        <div class="product-card-right">
          <h1
            class="product-title"
            :class="
              products?.data?.category === 'men\'s clothing'
                ? 'color-darkBlue'
                : 'color-darkPurple'
            "
          >
            {{ products?.data?.title }}
          </h1>

          <div class="product-information">
            <p class="product-category">{{ products?.data?.category }}</p>
            <div class="product-rating">
              <p class="product-rating_rate">
                {{ products?.data?.rating?.rate }} / 5
              </p>
              <div class="product-rating_circle">
                <div
                  v-for="rate in rates"
                  :key="rate"
                  class="circle"
                  :class="
                    products?.data?.category === 'men\'s clothing'
                      ? 'bg-darkBlue'
                      : 'bg-darkPurple'
                  "
                ></div>
              </div>
            </div>
          </div>
          <hr />

          <p class="product-description">{{ products?.data?.description }}</p>

          <div class="down">
            <hr />
            <h2
              class="product-price"
              :class="
                products?.data?.category === 'men\'s clothing'
                  ? 'color-darkBlue'
                  : 'color-darkPurple'
              "
            >
              ${{ products?.data?.price }}
            </h2>

            <div class="btn_container">
              <button
                class="btn"
                :class="
                  products?.data?.category === 'men\'s clothing'
                    ? 'font-white bg-darkBlue'
                    : 'font-white bg-darkPurple'
                "
              >
                Buy Now
              </button>
              <button
                @click="nextProduct()"
                class="btn btn-outline"
                :class="
                  products?.data?.category === 'men\'s clothing'
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
  </div>
</template>

<script>
import SkeletonDisplay from "./SkeletonDisplay.vue";
import { SERVICES } from "../configs";

export default {
  name: "ProductDisplayComponent",
  components: {
    SkeletonDisplay,
  },

  data() {
    return {
      index: 0,
      products: {},
      rates: [1, 2, 3, 4, 5],
      loading: false,
      productAvailable: false,
    };
  },

  methods: {
    async FetchAPI() {
      try {
        const url = `${SERVICES.GET_PRODUCT}/${this.index}`;
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error("Request Failed");
        }
        const result = await response.json();
        return result;
      } catch (error) {
        this.error = error.message;
      }
    },

    async nextProduct() {
      this.loading = true;

      if (this.index !== 20) {
        this.index++;
      } else {
        this.index = 1;
      }

      const data = await this.FetchAPI();
      if (
        data.category === "men's clothing" || data.category === "women's clothing"
      ) {
        this.products = { data };
        this.productAvailable = true;
      } else {
        this.productAvailable = false;
      }

      this.loading = false;
    },
  },

  mounted() {
    this.nextProduct();
  },
};
</script>

<style>
@import url("../assets/style/page.css");
</style>
