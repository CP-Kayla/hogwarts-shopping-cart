<template>
    <div>
      <h2>Order summary</h2>
      <button class="toggle-details-button" @click="hideDetails = !hideDetails">
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
  </template>
  
  <script setup>
  import { computed, ref } from 'vue'
  
  let hideDetails = ref(false)
  const { cartItems } = defineProps(['cartItems'])
  
  let subtotal = computed(() =>
    cartItems.reduce((acc, item) => acc + item.price * item.quantity, 0)
  )
  let shippingEstimate = computed(() => (subtotal.value > 10000 ? 100 : 50))
  let taxEstimate = computed(() => subtotal.value * 0.08)
  let total = computed(
    () => subtotal.value + shippingEstimate.value + taxEstimate.value
  )
  </script>
  
  <style scoped>
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