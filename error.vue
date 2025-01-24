<template>
    <div class="error-page">
      <h1 v-if="error.statusCode === 404">404 - 頁面找不到</h1>
      <h1 v-else>出錯了！</h1>
      <p>{{ error.message || '抱歉，我們找不到您需要的頁面。' }}</p>
      <NuxtLink to="/">返回首頁</NuxtLink>
    </div>
  </template>
  
  <script setup>
  defineProps(['error'])
  </script>
  
  <style>
  .error-page {
    text-align: center;
    padding: 2rem;
    font-family: Arial, sans-serif;
  }
  p {
    margin: 1rem 0;
    color: #555;
  }
  
  a {
    text-decoration: underline;
    color: #1e90ff;
  }
  </style>
  