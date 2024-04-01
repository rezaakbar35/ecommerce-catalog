<template>
  <div class="container">
    <div v-if="loading" class="skeleton">
      <div class="skeleton-image">
        <div class="loader"></div>
      </div>
      <div class="skeleton-content">
        <div class="skeleton-title"></div>
        <div class="skeleton-category"></div>
        <div class="skeleton-description"></div>
        <div class="skeleton-price"></div>
        <div class="skeleton-buttons">
          <div></div>
          <div></div>
        </div>
      </div>
    </div>
    <div
      v-else-if="
        product.category === 'women\'s clothing' ||
        product.category === 'men\'s clothing'
      "
      class="card"
    >
      <div class="image-container">
        <img class="image" :src="product.image" alt="" />
      </div>
      <div class="content">
        <div>
          <div class="title">{{ product.title }}</div>
          <div class="category">
            <div>{{ product.category }}</div>
            <div class="rating">
              <img
                src="https://img.icons8.com/fluency-systems-filled/48/star.png"
                alt="star"
              />
              <img
                src="https://img.icons8.com/fluency-systems-filled/48/star.png"
                alt="star"
              />
              <img
                src="https://img.icons8.com/fluency-systems-filled/48/star.png"
                alt="star"
              />
              <img
                src="https://img.icons8.com/fluency-systems-filled/48/star-half-empty.png"
                alt="star-half-empty"
              />
              <img
                src="https://img.icons8.com/fluency-systems-regular/48/star--v1.png"
                alt="star--v1"
              />
              <div>(3.5 / 5)</div>
            </div>
          </div>
          <div class="description">{{ product.description }}</div>
        </div>
        <div>
          <div class="price">${{ product.price }}</div>
          <div class="buttons">
            <div class="button1">Buy Now</div>
            <div class="button2" @click="getNextProduct">Next Product</div>
          </div>
        </div>
      </div>
    </div>
    <div v-else class="unavailable-card">
      <div class="unavailable-message">
        This product is unavailable to show.
      </div>
      <div class="button3" @click="getNextProduct">Next Product</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: true, // initial loading state
      productId: 0, // initial product ID
      product: {}, // empty object to store fetched product data
      backgrounds: {
        "men's clothing": "../assets/background-men.png",
        electronics: "../assets/background-men.png",
        "women's clothing": "../assets/background-women.png",
        jewelry: "../assets/background-women.png",
      },
    };
  },
  methods: {
    getNextProduct() {
      this.loading = true; // set loading state to true
      if (this.productId === 20) {
        this.productId = 1; // reset to product 1
      } else {
        this.productId++; // increment product ID
      }
      fetch(`https://fakestoreapi.com/products/${this.productId}`)
        .then((res) => res.json())
        .then((json) => {
          this.product = json; // update product data with fetched data
          console.log(json);
          document.body.classList.remove(
            "page-men",
            "page-women",
            "page-unavailable"
          );
          document.body.classList.add(this.getClassForCategory(json.category));
        })
        .finally(() => {
          this.loading = false; // set loading state to false
        });
    },
    getClassForCategory(category) {
      if (category === "men's clothing") {
        return "page-men";
      } else if (category === "women's clothing") {
        return "page-women";
      } else {
        return "page-unavailable";
      }
    },
  },

  mounted() {
    // Fetch initial product data when component is mounted
    this.getNextProduct();
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-position: top;
  background-repeat: no-repeat;
  background-size: 100vw 60vh;
}

.page-men {
  background-image: url("../assets/background-men.png");
}

/* Gayakan untuk halaman produk pakaian wanita */
.page-women {
  background-image: url("../assets/background-women.png");
}

/* Gayakan untuk halaman produk yang tidak tersedia */
.page-unavailable {
  background-image: url("../assets/background-unavailable.png");
}

.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  width: 75vw;
  height: 75vh;
  background-color: white;
  border-radius: 20px;
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.2), 0px 6px 6px rgba(0, 0, 0, 0.23);
  display: grid;
  grid-template-columns: 1fr 1fr;
  padding: 40px;
}

.unavailable-card {
  width: 75vw;
  height: 75vh;
  background-color: white;
  border-radius: 20px;
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.2), 0px 6px 6px rgba(0, 0, 0, 0.23);
  padding: 40px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-image: url("../assets/sad-face.png");
  background-position: center;
  background-repeat: no-repeat;
}

.image-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.image {
  max-width: 500px;
  max-height: 500px;
  margin-left: -50px;
}

.content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.title {
  font-size: 2rem;
  margin-bottom: 20px;
}

.category {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  margin-bottom: 20px;
}

.rating {
  display: flex;
  align-items: center;
}

.rating img {
  width: 20px;
}

.description {
  height: 240px;
  overflow-y: scroll;
  text-align: justify;
}

.price {
  font-size: 1.5rem;
  border-top: 1px solid rgba(0, 0, 0, 0.2);
  padding-top: 20px;
}

.buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}

.button1 {
  width: calc(50% - 5px);
  border: 1px solid black;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  color: white;
  background-color: black;
  cursor: pointer;
}

.button2 {
  width: calc(50% - 5px);
  border: 1px solid black;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  cursor: pointer;
}

.button3 {
  width: calc(50% - 30px);
  border: 1px solid black;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 5px;
  cursor: pointer;
}

.button:first-child {
  margin-right: 10px;
}

.unavailable-message {
  margin-bottom: 20px;
  margin-top: -50px;
  font-size: 24px;
}

.page-men .title {
  color: #002772;
}

.page-men .price {
  color: #002772;
}

.page-men .button1 {
  background-color: #002772;
}

.page-men .button2 {
  border-color: #002772;
  color: #002772;
}

/* Untuk halaman produk pakaian wanita */
.page-women .title {
  color: #720060;
}

.page-women .price {
  color: #720060;
}

.page-women .button1 {
  background-color: #720060;
}

.page-women .button2 {
  border-color: #720060;
  color: #720060;
}

.skeleton {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 75vw;
  height: 75vh;
  background-color: white;
  border-radius: 20px;
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.2), 0px 6px 6px rgba(0, 0, 0, 0.23);
  display: grid;
  grid-template-columns: 1fr 1fr;
  padding: 40px;
}

.skeleton-image {
  width: 400px;
  height: 500px;
  background-color: #ddd;
  border-radius: 10px;
  margin-left: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.loader {
  border: 4px solid #f3f3f3; /* Light grey */
  border-top: 4px solid #6d6868; /* Blue */
  border-radius: 50%;
  width: 100px;
  height: 100px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.skeleton-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.skeleton-title {
  width: 50%;
  height: 40px;
  background-color: #ddd;
  margin-bottom: 30px;
  animation: pulse 1s infinite;
}

.skeleton-category {
  width: 30%;
  height: 24px;
  background-color: #ddd;
  margin-bottom: 30px;
  animation: pulse 1s infinite;
}

.skeleton-description {
  width: 100%;
  height: 100px;
  background-color: #ddd;
  margin-bottom: 290px;
  animation: pulse 1s infinite;
}

.skeleton-price {
  width: 20%;
  height: 30px;
  background-color: #ddd;
  margin-bottom: 30px;
  animation: pulse 1s infinite;
}

.skeleton-buttons {
  display: flex;
  justify-content: space-between;
}

.skeleton-buttons div {
  width: calc(50% - 5px);
  height: 48px;
  background-color: #ddd;
  border-radius: 5px;
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0% {
    opacity: 50%;
  }
  50% {
    opacity: 100%;
  }
  100% {
    opacity: 50%;
  }
}
</style>
