<template>
    <div class=" bg-green-400">
        <div class="flex py-12 items-center">
            <div class="px-4 flex-none">
                <Avatar class="bg-gray-200"/>
            </div>
            <div class="flex flex-col flex-1">
                <p>{{ $t('profile.nickname') }}</p>
                <div class="flex">
                        <p class="flex-1">{{ $t('profile.userId') }}</p>
                            <div class="flex-none">
                                <button class="flex items-center rounded-x-3xl rounded-l-3xl text-slate-50 text-xs bg-green-300 p-1 "
                                @click="navigateToEdit">
                                    <Icon name="mdi-rename-box-outline" class="mr-1 w-4 h-4"/>
                                    {{ $t('profile.editProfile') }}
                                </button>
                            </div>
                </div>
            </div>
        </div>
        <div class="bg-gray-100 rounded-t-3xl shadow h-screen">
            <div class="flex p-4 justify-between">
               <button
               v-for="status in BookingStatus"::key="status.name"
               @click="selectStatus(status.name,status.route)"
               class=" flex flex-col items-center mx-auto text-sm"
               >
                <Icon :name="status.icon"
                class="w-6 h-6"
                ></Icon>
                {{ $t(`bookingStatus.${status.name}`) }}
               </button>
            </div>

            <div class="flex flex-col p-1 bg-white rounded-lg mx-4">
                <div class="border-b flex p-2 items-center "
                    v-for="settings in ProfileSettings"::key="settings.name"
                    @click="selectStatus(settings.name,settings.route)"
                >
                    <div class="flex flex-1 ">
                        <div class="flex flex-1 text-sm ">
                            <Icon :name = "settings.icon" class="w-5 h-5 mr-2"/>
                            {{ $t('profileSettings.' + settings.name) }}
                        </div>
                        <Icon name="ic-baseline-greater-than" class="w-4 h-4 bg-gray-300"></Icon>  
                    </div>
                </div>
                <div>
                    <div class=" flex p-3 items-center  ">
                            <div class="flex flex-1 text-sm -400 justify-center">
                                <Icon name = "solar-logout-2-outline" class="w-5 h-5 mr-2 "/>
                                <p>{{ $t('profile.logout') }}</p>
                            </div>
                    </div>
                </div>
            </div>
        </div>
    </div> 
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from 'vue-router';

const localPath = useLocalePath();
// 定義導航項目
const BookingStatus = [
  { name: "toPay", icon: "material-symbols-light-credit-card-clock-outline", route: "/" },
  { name: "notTraveled", icon: "material-symbols-calendar-clock-outline-sharp", route: "/" },
  { name: "refunded", icon: "tabler-credit-card-refund", route: "/" },
  { name: "all", icon: "material-symbols-lab-profile-outline", route: "/profile" },
];

const ProfileSettings =[
    {name:"changePassword", icon:"material-symbols-lock-outline-sharp",route:"/"},
    {name:"systemSettings", icon:"hugeicons-settings-03",route:"/"},
    {name:"contactSupport", icon:"material-symbols-call-log-outline",route:"/"},
    {name:"feedback", icon:"material-symbols-mail-outline",route:"/"},
]

// 切換選中的導航按鈕
const selectStatus = (statusName: string, route: string) => {
  navigateTo(route); // 使用 Nuxt 的 `navigateTo` 方法進行導航
};

const router = useRouter();

const navigateToEdit = () => {
    const path = localPath('/editProfile');
    router.push(path);
};
</script>