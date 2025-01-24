<script setup lang="ts">
import LangSwither from './LangSwitcher.vue';
</script>

<template>
  <header class="py-4 border-b bg-green-400 flex items-center ">
    <div>
        <LangSwitcher/>
      </div>
      <h1 class="absolute left-1/2 transform -translate-x-1/2 text-xl font-bold capitalize ">
        {{ $t('title') }}
      </h1>
  </header>
</template>