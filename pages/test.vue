<template>
   <div class="flex flex-col">
    <!-- 頂部標題 -->
      <header class="py-6 border-b bg-green-400 flex items-center ">
        <h1 class="absolute left-1/2 transform-translate-x-1/2 text-xl font-bold capitalize">
          {{ $t("Management.orderManagement") }}
        </h1>
      </header>

    <div class="flex">
      <!-- 側邊欄 -->
      <div class="bg-stone-50 shadow-md flex flex-col h-auto">
        <div
          v-for="option in options"
          :key="option.name"
          @click="selectedoption = option.name"
          class="flex flex-col py-4 items-center text-sm shadow-sm px-4 cursor-pointer transition-all duration-200"
          :class="selectedoption === option.name ? 'text-green-500 font-bold bg-teal-50' : 'text-gray-500 hover:text-green-400'"
        >
          <Icon :name="option.icon" class="h-6 w-6" />
          <span class="ml-1">{{ $t(`Management.${option.name}`) }}</span>
        </div>
      </div>

      <!-- 主要內容區 -->
      <div class="flex-1 flex flex-col">
        <!-- 標題 + 按鈕 -->
        <div class="pt-2 pl-4 bg-sky-50 flex justify-between items-center">
          <h2 class="font-bold pb-2">{{ $t(titleKey) }}</h2>
          <button 
            v-if="buttonText" 
            :class="buttonClass"
            class="shadow-md rounded-xl px-2 mx-2 mb-2 text-white"
          >
            {{ $t(buttonText) }}
          </button>
        </div>

        <!-- 搜索框 -->
         <div class="bg-white px-4 py-2 ">
           <div class="flex items-center border rounded-md w-1/3">
             <Icon name="material-symbols-search-rounded" class="h-5 w-5 text-gray-500 mx-2" /></input>
             <input
             type="text"
             class=" py-1 w-full rounded-md focus:ring-0 focus:border-transparent border-none outline-none"
             :placeholder="$t('Management.searchOrder')"
             >
           </div>
         </div>
        <div class=" bg-white h-full px-4 py-2">
          <div class="overflow-x-auto">
            <table class="table-auto w-full border-collapse border border-gray-200">
              <thead>
                <tr class="bg-gray-100">
                  <th v-for="(header, index) in translatedHeaders" :key="index" class="border">
                    {{ header }}
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(order, index) in filteredOrders" :key="index" class="border-b">
                  <td class="border p-2">{{ order.id }}</td>
                  <td class="border p-2">{{ order.ticketCode }}</td>
                  <td class="border p-2">{{ order.pickup }}</td>
                  <td class="border p-2">{{ order.dropoff }}</td>
                  <td class="border p-2">{{ order.time }}</td>
                  <td class="border p-2">
                    <span :class="statusClass(order.status)">{{ order.status }}</span>
                  </td>
                  <td class="border p-2">{{ order.fullTickets }}</td>
                  <td class="border p-2">{{ order.price }}</td>
                  <td class="border p-2">{{ order.date }}</td>
                  <td class="border p-2">{{ order.contact }}</td>
                  <td class="border p-2">{{ order.phone }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
  
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import { useI18n } from 'vue-i18n'

const { t } = useI18n();
const searchQuery = ref('')

// 訂單數據
const orders = ref([
  { id: '20250128000207', ticketCode: '4482451', pickup: '水头码头', dropoff: '尚义机场', time: '2025/2/6 10:00:00', status: '未出行', fullTickets: 2, price: 50, date: '2025/1/28 14:49:21', contact: '张三', phone: '1234567890' },
  { id: '20250203000148', ticketCode: '9367772', pickup: '水头码头', dropoff: '尚义机场', time: '2025/2/6 10:00:00', status: '已完成', fullTickets: 1, price: 30, date: '2025/2/3 11:17:14', contact: '李四', phone: '0987654321' }
]);

// 使用 i18n 獲取表頭名稱
const translatedHeaders = computed(() => [
  t("Management.orderNumber"),
  t("Management.boardingCode"),
  t("Management.boardingPoint"),
  t("Management.dropOffPoint"),
  t("Management.rideTime"),
  t("Management.status"),
  t("Management.fullTicketCount"),
  t("Management.unitPrice"),
  t("Management.orderDate"),
  t("Management.orderContact"),
  t("Management.orderPhoneNumber")
]);

// 篩選訂單
const filteredOrders = computed(() => {
  return orders.value.filter(order => 
    Object.values(order).some(value => 
      String(value).includes(searchQuery.value)
    )
  );
});

// 設置狀態顏色
const statusClass = (status: string) => ({
  'text-green-600 font-bold': status === '已完成',
  'text-red-600 font-bold': status === '未出行',
  'text-yellow-600 font-bold': status === '处理中'
});
// 定義可選項目的類型
type OptionKey = "viewOrder" | "printOrder" | "addOrder" | "deleteOrder" | "editOrder";

const selectedoption = ref<OptionKey>("viewOrder");

// 側邊欄導航選項
const options: { name: OptionKey; icon: string }[] = [
  { name: "viewOrder", icon: "material-symbols-grid-view-outline-rounded" },
  { name: "printOrder", icon: "material-symbols-print-outline" },
  { name: "addOrder", icon: "material-symbols-add-diamond-outline" },
  { name: "deleteOrder", icon: "material-symbols-delete-outline" },
  { name: "editOrder", icon: "material-symbols-edit-square-outline" }
];

// 定義標題映射
const titles: Record<OptionKey, string> = {
  viewOrder: "Management.viewOrder",
  printOrder: "Management.printOrder",
  addOrder: "Management.addOrder",
  deleteOrder: "Management.deleteOrder",
  editOrder: "Management.editOrder"
};

// 定義按鈕映射（部分選項沒有按鈕）
const buttons: Partial<Record<OptionKey, { text: string; class: string }>> = {
  printOrder: { text: "Management.print", class: "bg-blue-400" },
  addOrder: { text: "Management.add", class: "bg-green-500" },
  deleteOrder: { text: "Management.delete", class: "bg-red-500" },
  editOrder: { text: "Management.modify", class: "bg-yellow-400" }
};

// 計算標題與按鈕
const titleKey = computed(() => titles[selectedoption.value]);
const buttonText = computed(() => buttons[selectedoption.value]?.text ?? "");
const buttonClass = computed(() => buttons[selectedoption.value]?.class ?? "");
</script>
