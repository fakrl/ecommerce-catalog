<template>
  <div
    class="product-container"
    :class="{
      'men-section': product && product.category === 'men\'s clothing',
      'women-section': product && product.category === 'women\'s clothing',
      'unavailable-section': !product && !isLoading,
    }"
  >
    <!-- Loading State -->
    <div v-if="isLoading" class="loader">
      <div class="spinner"></div>
      <p>Loading product...</p>
    </div>

    <!-- Men's Clothing -->
    <div
      v-else-if="product && product.category === 'men\'s clothing'"
      class="product-card"
    >
      <h2 class="product-title">{{ product.title }}</h2>
      <p class="category-badge">{{ product.category }}</p>
      <p class="rating">
        <span class="rating-stars">⭐</span>
        {{ product.rating.rate }}/5 ({{ product.rating.count }} reviews)
      </p>

      <img :src="product.image" :alt="product.title" class="product-image" />

      <p class="product-description">{{ product.description }}</p>

      <p class="price">${{ product.price }}</p>

      <div class="button-group">
        <button class="btn-buy">Buy now</button>
        <button class="btn-next" @click="nextProduct">Next product</button>
      </div>
    </div>

    <!-- Women's Clothing -->
    <div
      v-else-if="product && product.category === 'women\'s clothing'"
      class="product-card"
    >
      <h2 class="product-title">{{ product.title }}</h2>
      <p class="category-badge">{{ product.category }}</p>
      <p class="rating">
        <span class="rating-stars">⭐</span>
        {{ product.rating.rate }}/5 ({{ product.rating.count }} reviews)
      </p>

      <img :src="product.image" :alt="product.title" class="product-image" />

      <p class="product-description">{{ product.description }}</p>

      <p class="price">${{ product.price }}</p>

      <div class="button-group">
        <button class="btn-buy">Buy now</button>
        <button class="btn-next" @click="nextProduct">Next product</button>
      </div>
    </div>

    <!-- Unavailable Product -->
    <div v-else class="unavailable-card">
      <h2 class="unavailable-title">This product is unavailable to show</h2>
      <p class="unavailable-text">
        We're sorry, this product is not in our clothing collection.
      </p>
      <button class="btn-next" @click="nextProduct">Next product</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      currentIndex: 1,
      product: null,
      isLoading: false,
    };
  },
  methods: {
    async fetchProduct() {
      this.isLoading = true;
      this.product = null;

      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.currentIndex}`
        );
        const data = await response.json();

        console.log("Fetched:", data.title, "- Category:", data.category);

        if (
          data.category === "men's clothing" ||
          data.category === "women's clothing"
        ) {
          this.product = data;
        } else {
          this.product = null;
        }
      } catch (error) {
        console.error("Error fetching product:", error);
        this.product = null;
      } finally {
        this.isLoading = false;
      }
    },

    nextProduct() {
      this.currentIndex++;

      if (this.currentIndex > 20) {
        this.currentIndex = 1;
      }

      this.fetchProduct();
    },
  },
  mounted() {
    this.fetchProduct();
  },
};
</script>

<style scoped>
@import "@/assets/style/page.css";
</style>
