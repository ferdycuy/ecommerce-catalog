
<template>
  <!-- loading spinner -->
  <div v-if="isLoading" class="loading-container">
      <div class="loading-spinner"></div>
  </div>
  <!-- akhir loading spinner -->

  <div class="card"
      :class="product.category === 'men\'s clothing' ? 'bg-men' : product.category === 'women\'s clothing' ? 'bg-women' : 'bg-unavailable'">
      <div class="images">
           <img src="../assets/bg-pattern.png">
       </div>
      <!-- Produk yang tersedia -->
      <div v-if="product.category === 'men\'s clothing' || product.category === 'women\'s clothing'" class="product-card">
          <div class="product-img">
              <img :src="product.image" alt="product-image" width="68%">
          </div>
          <div class="product-desc">
              <h1 class="title" :class="product.category === 'men\'s clothing' ? 'text-dark-blue' : 'text-purple'">
                  {{ product.title }}
              </h1>
              <div class="category-rating">
                  <p>{{ product.category }}</p>
                  <div class="rating"><span class="rating-num">{{ product.rating?.rate }}/5</span>
                      <div v-for="rate in rating" :key="rate">
                          <div :class="product.category === 'men\'s clothing' ? 'circle-men' : 'circle-women'">
                          </div>
                      </div>
                  </div>
              </div>
              <p class=" desc">{{ product.description }}</p>
              <h1 class="price" :class="product.category === 'men\'s clothing' ? 'text-dark-blue' : 'text-purple'">
                  ${{ product.price }}
              </h1>
              <div class="btn-group">
                  <button class="btn" :class="product.category === 'men\'s clothing' ? 'btn-buy-men' : 'btn-buy-women'">
                      Buy now
                  </button>
                  <button @click="getProductById" class="btn"
                      :class="product.category === 'men\'s clothing' ? 'btn-next-men' : 'btn-next-women'">
                      Next product
                  </button>
              </div>
          </div>
      </div>
      <!-- akhir dari produk yang tersedia -->

      <!-- Produk tidak tersedia (unavailable) -->
      <div v-else class="product-card product-unavailable">
          <p class="text-unavailable">This product is unavailable to show</p>
          <button @click="getProductById" class="btn btn-next-unavailable">
              Next product
          </button>
      </div>
      <!-- akhir dari produk tidak tersedia (available)-->

  </div>
</template> 

<script>
export default {
  name: "ProductDisplay",
  data() {
      return {
          index: 0,
          product: {},
          rating: [1, 2, 3, 4, 5],
          isLoading: false
      }
  },
  methods: {
      async getProductById() {
          this.isLoading = true
          this.index >= 20 ? this.index = 1 : this.index++

          // fetch data
          const response = await fetch(`https://fakestoreapi.com/products/${this.index}`)
          const data = await response.json()

          this.product = data // data.data
          this.isLoading = false
      }
  },
  mounted() {
      this.getProductById()
  }
}
</script>

<style>
@import '../assets/style/page.css';
</style>