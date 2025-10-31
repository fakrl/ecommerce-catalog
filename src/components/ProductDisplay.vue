<template>
  <div class="product-container">
    <!-- Loading State -->
    <div v-if="isLoading" class="loader">
      <div class="spinner"></div>
      <p>Loading product...</p>
    </div>

    <!-- Product Display -->
    <div v-else-if="product" class="product-display">
      <h2>{{ product.title }}</h2>
      <img :src="product.image" :alt="product.title" />
      <p class="category">Category: {{ product.category }}</p>
      <p class="price">${{ product.price }}</p>
      <p class="description">{{ product.description }}</p>
      <p class="rating">
        ⭐ {{ product.rating.rate }} ({{ product.rating.count }} reviews)
      </p>
    </div>

    <!-- Unavailable Product -->
    <div v-else class="unavailable">
      <h2>Product Not Available</h2>
      <p>This product is not in our clothing collection.</p>
    </div>

    <!-- Next Product Button -->
    <button @click="nextProduct" class="next-btn">Next Product</button>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      currentIndex: 1, // Index produk (1-20)
      product: null, // Data produk yang ditampilkan
      isLoading: false, // Status loading
    };
  },
  methods: {
    async fetchProduct() {
      this.isLoading = true; // Mulai loading
      this.product = null; // Reset product

      try {
        // Fetch dari API
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.currentIndex}`
        );
        const data = await response.json();

        console.log("Fetched data:", data); // Buat debugging

        // Cek category
        if (
          data.category === "men's clothing" ||
          data.category === "women's clothing"
        ) {
          // Kalo men's atau women's → simpan
          this.product = data;
        } else {
          // Kalo bukan clothing → product tetep null
          this.product = null;
        }
      } catch (error) {
        console.error("Error fetching product:", error);
        this.product = null;
      } finally {
        this.isLoading = false; // Stop loading
      }
    },

    nextProduct() {
      // Increment index
      this.currentIndex++;

      // Reset ke 1 kalo udah 20
      if (this.currentIndex > 20) {
        this.currentIndex = 1;
      }

      // Fetch product baru
      this.fetchProduct();
    },
  },
  mounted() {
    // Fetch product pertama kali pas component di-load
    this.fetchProduct();
  },
};
</script>

<style scoped>
.product-container {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
}

/* Loading Spinner */
.loader {
  padding: 50px;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Product Display */
.product-display img {
  max-width: 300px;
  height: auto;
  margin: 20px 0;
}

.category {
  color: #666;
  font-style: italic;
}

.price {
  font-size: 24px;
  font-weight: bold;
  color: #27ae60;
  margin: 10px 0;
}

.description {
  text-align: left;
  line-height: 1.6;
  color: #555;
}

.rating {
  color: #f39c12;
  font-weight: bold;
}

/* Unavailable */
.unavailable {
  padding: 50px;
  color: #e74c3c;
}

/* Button */
.next-btn {
  background-color: #3498db;
  color: white;
  padding: 15px 30px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 30px;
}

.next-btn:hover {
  background-color: #2980b9;
}
</style>
