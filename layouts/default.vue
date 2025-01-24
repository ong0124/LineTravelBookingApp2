<script setup lang="ts">
import { useRoute } from 'vue-router';
const route = useRoute();
const hideBottomNavigatorRoutes = ['confirmationPage','payment'];
const shouldShowBottomNavigator = !hideBottomNavigatorRoutes.includes(route.name as string);
</script>

<template>
  <div class="bg-gray-100">
    <slot />
    <BottomNavigator v-if="shouldShowBottomNavigator" />
  </div>
</template>