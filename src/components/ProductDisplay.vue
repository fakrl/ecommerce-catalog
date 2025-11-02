<template>
  <div
    class="product-container"
    :class="{
      'men-section': product && product.category === `men's clothing`,
      'women-section': product && product.category === `women's clothing`,
      'unavailable-section': !product && !isLoading,
    }"
  >
    <!-- Loading State -->
    <div v-if="isLoading" class="loader">
      <div class="spinner"></div>
      <p>Loading product...</p>
    </div>

    <!-- Product Display -->
    <div v-else-if="product" class="product-card">
      <!-- Product Image -->
      <div class="product-image-wrapper">
        <img :src="product.image" :alt="product.title" class="product-image" />
      </div>

      <!-- Product Info -->
      <div class="product-info">
        <h2 class="product-title">{{ product.title }}</h2>

        <!-- Category & Rating Row -->
        <div class="category-rating-row">
          <p class="category-badge">{{ product.category }}</p>
          <div class="rating">
            <span>{{ product.rating.rate }}/5</span>
            <div class="rating-stars">
              <span
                v-for="n in 5"
                :key="n"
                class="star"
                :class="{
                  filled: n <= Math.round(product.rating.rate),
                  empty: n > Math.round(product.rating.rate),
                }"
              ></span>
            </div>
          </div>
        </div>

        <!-- Description -->
        <p class="product-description">{{ product.description }}</p>

        <!-- Price -->
        <p class="price">${{ product.price }}</p>

        <!-- Buttons -->
        <div class="button-group">
          <button class="btn-buy">Buy now</button>
          <button class="btn-next" @click="nextProduct">Next product</button>
        </div>
      </div>
    </div>

    <!-- Unavailable Section -->
    <div v-else class="unavailable-card">
      <div class="unavailable-content">
        <h3 class="unavailable-title">This product is unavailable to show</h3>
        <button class="btn-next" @click="nextProduct">Next product</button>
      </div>
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
      if (this.currentIndex > 20) this.currentIndex = 1;
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

/* Override unavailable section untuk pasang SVG di dalam card */
.unavailable-card {
  position: relative;
  background: var(--white) url("@/assets/images/sad-face.svg") no-repeat center
    70%;
  background-size: 70%;
  border-radius: 10px;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  padding: 100px 60px;
  max-width: 1034px;
  width: calc(100% - 40px);
  margin: 0 20px;
  min-height: 580px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.unavailable-content {
  position: relative;
  z-index: 10;
}
</style>
