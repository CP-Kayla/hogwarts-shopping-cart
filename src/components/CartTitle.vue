<template>
    <h1>{{ username }}'s Shopping Cart</h1>
  </template>
  
  <script setup>
  defineProps(['username'])
  </script>
  
  <style scoped>
  /* Styles for the cart title */
  h1 {
    padding: 20px;
    max-width: 1200px;
    margin: auto;
  }
  </style>