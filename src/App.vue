<template>
  <div class="shopping-cart">
    <h1>{{ username }}'s Shopping Cart</h1>
    <div class="cart-container">
      <div class="cart-list">
        <div
          class="cart-list-item"
          v-for="item in shoppingCartItems"
          :key="item.id"
        >
          <img
            :src="item.image"
            :alt="item.productName"
            class="product-image"
          />
          <div class="item-details-with-actions">
            <div class="item-details">
              <h2>{{ item.productName }}</h2>
              <p class="price">${{ item.price }}</p>
              <p class="in-stock-status" v-if="item.isInStock">
                <i class="fa-solid fa-check"></i> In stock
              </p>
              <p class="on-backorder-status" v-else>
                <i class="fa-solid fa-hourglass-half"></i> On backorder
              </p>
            </div>
            <div class="item-actions">
              <div class="quantity-selector">
                <button
                  class="quantity-change-button"
                  @click="decreaseOne(item.id)"
                >
                  −
                </button>
                <input
                  type="text"
                  class="quantity-input"
                  v-model.number="item.quantity"
                  aria-label="quantity"
                />
                <button
                  class="quantity-change-button"
                  @click="increaseOne(item.id)"
                >
                  +
                </button>
              </div>
              <button class="remove-item" @click="removeItem(item.id)">
                ✕
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="order-summary">
        <h2>Order summary</h2>
        <button
          class="toggle-details-button"
          @click="hideDetails = !hideDetails"
        >
          {{ hideDetails ? 'Show Details' : 'Hide Details' }}
        </button>
        <div :class="{ 'hide-order-details': hideDetails }">
          <div class="summary-item">
            <span>Subtotal</span>
            <span>${{ subtotal }}</span>
          </div>
          <div class="summary-item">
            <span>Shipping estimate</span>
            <span>${{ shippingEstimate }}</span>
          </div>
          <div class="summary-item">
            <span>Tax estimate</span>
            <span>${{ taxEstimate }}</span>
          </div>
        </div>
        <div class="summary-total">
          <strong>Order total</strong>
          <strong>${{ total }}</strong>
        </div>
        <button class="checkout-button">Checkout</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue'

let username = 'Harry'
let shoppingCartItems = ref([
  {
    id: 1,
    productName: 'Dragon Liver',
    price: 1500,
    isInStock: true,
    quantity: 3,
    image: 'src/assets/img/DragonLiver.png'
  },
  {
    id: 2,
    productName: 'Golden Snitch',
    price: 600,
    isInStock: true,
    quantity: 2,
    image: ' src/assets/img/GoldenSnitch.png'
  },
  {
    id: 3,
    productName: 'Unicorn Tail Hair',
    price: 1200,
    isInStock: false,
    quantity: 1,
    image: 'src/assets/img/UnicornTailHair.png'
  },
  {
    id: 4,
    productName: 'Wand',
    price: 2000,
    isInStock: true,
    quantity: 1,
    image: 'src/assets/img/Wand.jpg'
  },
  {
    id: 5,
    productName: 'Nimbus 2000',
    price: 5000,
    isInStock: true,
    quantity: 1,
    image: 'src/assets/img/Nimbus2000.jpg'
  }
])

let hideDetails = ref(false)

function decreaseOne(id) {
  shoppingCartItems.value.some((item) => {
    if (item.id == id && item.quantity != 0) {
      item.quantity = item.quantity - 1
    }
  })
}

function increaseOne(id) {
  shoppingCartItems.value.some((item) => {
    if (item.id == id) {
      item.quantity = item.quantity + 1
    }
  })
}

function removeItem(id) {
  // Step 1: find the index of the item to be deleted
  let index = shoppingCartItems.value.findIndex((item) => {
    return item.id == id
  })
  // Step 2: delete this item from the list
  shoppingCartItems.value.splice(index, 1)
}
let subtotal = computed(() =>
  shoppingCartItems.value.reduce(
    (acc, item) => acc + item.price * item.quantity,
    0
  )
)

let shippingEstimate = computed(() => (subtotal.value > 10000 ? 100 : 50))

let taxEstimate = computed(() => subtotal.value * 0.08)

let total = computed(
  () => subtotal.value + shippingEstimate.value + taxEstimate.value
)

// Automatically save to localStorage whenever any part of shoppingCartItems changes
watch(
  shoppingCartItems,
  () => {
    localStorage.setItem(
      'hogwartsShoppingCart',
      JSON.stringify(shoppingCartItems.value)
    )
  },
  { deep: true }
)
</script>

<style scoped>
/* Styles for the shopping cart */
.shopping-cart {
  font-family: 'Arial', sans-serif;
  background-color: #f8f8f8;
  margin: 0;
  padding: 0;
}

/* Styles for the cart title */
h1 {
  padding: 20px;
  max-width: 1200px;
  margin: auto;
}

/* Styles for the cart list and order summary */
.cart-container {
  display: flex;
  align-items: flex-start;
  max-width: 1200px;
  margin: auto;
}

.cart-list {
  flex-grow: 2;
  margin-right: 20px;
}

.order-summary {
  flex-basis: 300px;
  background-color: #f9fafb;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Styles for the cart list item */
.cart-list-item {
  display: flex;
  align-items: center;
  background-color: #fff;
  margin-bottom: 10px;
  border-radius: 8px;
  padding: 10px;
}

.product-image {
  width: 100px;
  margin-right: 10px;
}

.item-details-with-actions {
  display: flex;
  flex-grow: 1;
  margin-left: 10px;
}

.item-details {
  flex-grow: 1;
}

.item-actions {
  display: flex;
  align-items: center;
}

.price {
  font-size: 0.9em;
  color: #666;
}

.in-stock-status {
  font-size: 0.9em;
  color: green;
}

.on-backorder-status {
  font-size: 0.9em;
  color: red;
}

.quantity-selector {
  display: flex;
  border: 1px solid #c1c1c1;
  border-radius: 8px;
  overflow: hidden;
  /* Ensures the children do not break the rounded corners */
}

.quantity-change-button {
  background-color: #ffffff;
  border: none;
  padding: 10px 12px;
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.2s;
}

.quantity-input {
  border: none;
  text-align: center;
  width: 40px;
  padding: 10px;
  font-size: 1rem;
  transition: all 0.2s;
}

.quantity-input:focus {
  outline: none;
}

.remove-item {
  background: none;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.2em;
  margin-left: 20px;
}

.quantity-change-button:hover,
.quantity-change-button:focus,
.quantity-input:focus,
.remove-item:hover,
.remove-item:focus {
  background-color: #f2f2f2;
}

/* Styles for the order summary */
.toggle-details-button {
  background-color: #f1f1f1;
  color: #333;
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-bottom: 10px;
}

.hide-order-details {
  display: none;
  /* Hide details by default */
}

.summary-item {
  display: flex;
  justify-content: space-between;
  padding: 10px 0;
}

.summary-total {
  display: flex;
  justify-content: space-between;
  font-weight: bold;
  padding: 10px 0;
  border-top: 1px solid #e2e2e2;
  margin-top: 10px;
}

.checkout-button {
  background-color: #4f46e5;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  width: 100%;
  cursor: pointer;
  margin-top: 10px;
  transition: all 0.2s;
  /* smooth transition in and out */
}

.checkout-button:hover {
  background-color: #4138d9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
</style>