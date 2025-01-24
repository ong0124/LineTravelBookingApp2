<template>
    <a-space direction="vertical" :size="12">
      <a-date-picker v-model:value="value1" />
    </a-space>
  </template>
  <script lang="ts" setup>
  import { ref } from 'vue';
  import type { Dayjs } from 'dayjs';
  const value1 = ref<Dayjs>();
  </script>
  
  