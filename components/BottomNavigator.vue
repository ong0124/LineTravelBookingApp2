

<template>
    <footer class="border-t bg-white fixed inset-x-0 bottom-0">
        <div class="flex justify-between">
            <button
             v-for="page in pages"
             :key="page.name"
             @click="selectPage(page.name, page.route)"
             class="flex flex-col items-center mx-auto text-sm">
                <Icon
                :name="page.icon"
                class="h-5 w-5"
                :class="selectedPage === page.name ? 'text-green-500' : 'text-gray-400'"
                />
                <span
                :class="selectedPage === page.name ? 'text-green-500 font-bold' : 'text-gray-500'"
                >
                {{ $t(`footer.${page.name}`) }}
                </span>
            </button>
        </div>
    </footer>
</template>


<script setup lang="ts">
import { ref } from "vue";
const localPath = useLocalePath();
// 定義當前選中的導航按鈕
const selectedPage = ref("shuttle");

// 定義導航項目
const pages = [
  { name: "shuttle", icon: "material-symbols-directions-car", route: "/" },
  { name: "charter", icon: "mdi-bus", route: "/" },
  { name: "pickup", icon: "mdi-car-search-outline", route: "/" },
  { name: "profile", icon: "material-symbols-person-2-outline", route: "/profile" },
];

// 切換選中的導航按鈕
const selectPage = (pageName: string, route: string) => {
  selectedPage.value = pageName;
  const selectedRoute = localPath(route);
  navigateTo(selectedRoute); // 使用 Nuxt 的 `navigateTo` 方法進行導航
};
</script>