<template>
    <div>
        <button @click="goBack" class="pl-2">
            <Icon name="tabler-arrow-back-up" class="mr-1 bg-white ml-4"/>
        </button>
    </div>
</template>


<script setup lang="ts">
import { useRouter } from 'vue-router';

const router = useRouter();

function goBack() {
  router.back();
}
</script>