<template>
    <div class="cart-list-item">
      <img :src="item.image" :alt="item.productName" class="product-image" />
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
              @click="updateQuantity(item.id, item.quantity - 1)"
            >
              −
            </button>
            <input
              type="text"
              class="quantity-input"
              :value="item.quantity"
              aria-label="quantity"
              @blur="updateQuantity(item.id, $event.target.value)"
            />
            <button
              class="quantity-change-button"
              @click="updateQuantity(item.id, item.quantity + 1)"
            >
              +
            </button>
          </div>
          <button class="remove-item" @click="$emit('item-remove', item.id)">
            ✕
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  defineProps(['item'])
  const emit = defineEmits(['item-remove', 'quantity-update'])
  
  function updateQuantity(id, newQuantity) {
    emit('quantity-update', { id, newQuantity })
  }
  </script>
  
  <style scoped>
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
  </style>