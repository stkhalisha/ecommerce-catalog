<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      product: {},
      index: 1,
      isLoading: false,
      isProductAvailable: true,
    };
  },
  methods: {
    async fetchProduct() {
      this.isLoading = true;
      await fetch(`https://fakestoreapi.com/products/${this.index}`)
        .then((response) => response.json())
        .then((data) => {
          if (
            data.category === "men's clothing" ||
            data.category === "women's clothing"
          ) {
            this.product = data;
            this.setProductClass(data.category);
          } else {
            this.fetchNextProduct();
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
    fetchNextProduct() {
      this.index = this.index === 20 ? 1 : this.index + 1;
      this.fetchProduct();
    },
    setProductClass(category) {
      if (category === "men's clothing") {
        this.productClass = "page-men";
      } else if (category === "women's clothing") {
        this.productClass = "page-women";
      } else {
        this.productClass = "page-unavailable";
      }
    },
  },
  mounted() {
    this.fetchProduct();
  },
};
</script>

<template>
  <body>
    <main
      class="productClass"
      :class="
        !isProductAvailable || isLoading
          ? 'bg-secondary-gray'
          : isProductAvailable && product.category === 'men\'s clothing'
          ? 'bg-pattern bg-light-blue'
          : 'bg-pattern bg-light-purple'
      "
    >
      <section class="product-container">
        <div class="left-card">
          <img :src="product.image" :alt="product.title" />
        </div>

        <div class="right-card">
          <div class="right-card-header">
            <h1 class="product-title">{{ product.title }}</h1>
            <div class="product-category-rating">
              <div>
                <p
                  class="product-category"
                  :class="{
                    category: product.category === 'men\'s clothing',
                    category: product.category === 'women\'s clothing',
                  }"
                >
                  {{ product.category }}
                </p>
              </div>

              <div class="product-rating">
                <p>4.7/5<span></span></p>
              </div>
            </div>
          </div>
          <div class="right-card-description">
            <p class="product-description">
              {{ product.description }}
            </p>
          </div>

          <div class="right-card-price">
            <h2 class="product-price">${{ product.price }}</h2>
            <button
              class="buy-btn"
              :class="{
                'buy-btn-men': product.category === 'men\'s clothing',
                'buy-btn-women': product.category === 'women\'s clothing',
              }"
            >
              Buy now
            </button>
            <button
              class="next-btn"
              :class="{
                'next-btn-men': product.category === 'men\'s clothing',
                'next-btn-women': product.category === 'women\'s clothing',
              }"
              @click="fetchNextProduct()"
            >
              Next Product
            </button>
          </div>
        </div>
      </section>
    </main>
  </body>
</template>

<style scoped>
@import "@/assets/style/page.css";
</style>
