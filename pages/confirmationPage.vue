<template>
<div class="h-screen bg-gray-200"> 
    <div class="h-1/3 bg-gradient-to-b from-green-400 ">
    <header class="flex items-center py-4">
        <BackBtn/>
        <h1 class="absolute left-1/2 transform -translate-x-1/2 text-base font-bold capitalize text-white ">
            {{ $t('confirmationPage.title') }}
        </h1>
     </header>
    <div class="mx-3 pt-4 pb-8 rounded-lg bg-white">
        <div class="px-4">
            <div class="px-2">
                <p class="border-l-4 border-orange-400 pl-2">
                    {{ $t('confirmationPage.tripInfo') }}
                </p>
                <p class="my-2">{{ DateShuttle }} {{ TimeShuttle }}</p>
                <div class="flex ">
                    <p>{{ translatedp2 }}</p>
                    <Icon name="material-symbols-arrow-right-alt" class="mx-2 w-6 h-6"></Icon>
                    <p>{{ translatedp3 }}</p>
                </div>
            </div>
           <DashLine/>  
           <div v-if="tab === 'roundedway'" class="px-2">
                <p class="border-l-4 border-orange-400 pl-2">
                    {{ $t('confirmationPage.returnTripInfo') }}
                </p>
                <p class="my-2">{{ DateShuttleReturn }} {{ TimeShuttleReturn }}</p>
                <div class="flex ">
                    <p>{{ translatedp4 }}</p>
                    <Icon name="material-symbols-arrow-right-alt" class="mx-2 w-6 h-6"></Icon>
                    <p>{{ translatedp5 }}</p>
                </div>
                <DashLine/>   
            </div>
                <div class="px-2">
                    <div class="flex py-2">
                        <p class="border-l-4 border-orange-400 pl-2 flex-1">
                            {{ $t('confirmationPage.OrderTotal') }}
                        </p>
                        <a href="#" class="text-sm text-gray-600 ml-1">{{ $t('confirmationPage.refundPolicy') }}</a>
                        </div>
                     <div class="flex">
                     <p class="flex-1">{{ $t('confirmationPage.adultTicket') }} {{ adult }}</p>           
                     <p>&yen; &nbsp;{{ totalPrice }}.00</p>
                        </div>
                    <div class="flex">
                    <p class="flex-1">{{ $t('confirmationPage.infantTicket') }} {{ child }}</p>
                    <p>&yen; &nbsp;0.00</p>
                    </div>
                </div>
           </div>
    </div>
    <div class="text-xs p-6 text-gray-700">
        <p>{{ $t('reminder.notice') }}</p>
        <p>{{ $t('reminder.tips1') }}</p>
        <p>{{ $t('reminder.tips2') }}</p>
        <p>{{ $t('reminder.tips3') }}</p>
        <p>{{ $t('reminder.tips4') }}</p>
        <p>{{ $t('reminder.tips5') }}</p>
    </div>

    <footer class="border-t bg-white fixed inset-x-0 bottom-6">
        <div class="py-6 mx-6 flex">
            <div class="flex flex-1">
                <p>{{ $t('confirmationPage.total') }} &nbsp;  &nbsp;</p>
                <p class="text-red-500">&yen; &nbsp;{{ totalPrice }}.00</p>
            </div>
            <NuxtLink :to="localPath('/payment')" class="bg-red-500 text-white rounded px-2 py-1">{{ $t('confirmationPage.submitOrder') }}</NuxtLink>
        </div>
    </footer>
</div>
</div>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';
import { useRoute } from 'vue-router';
import { useI18n } from 'vue-i18n';

export default defineComponent({
  setup() {
    const localPath = useLocalePath();
    const route = useRoute();
    const { t } = useI18n();
    
    const tab = (route.query.tab || '未提供');

    console.log('tab 的值:', tab);
    const adult = route.query.adult || '0';
    const child = route.query.child || '0';
    const totalPrice = route.query.totalPrice || '0';
    const DateArrivalShip = route.query.DateArrivalShip || '未提供';
    const DateShuttle = route.query.DateShuttle || '未提供';
    const TimeArrivalShip = route.query.TimeArrivalShip || '未提供';
    const TimeShuttle = route.query.TimeShuttle || '未提供';
    const phone = route.query.phone || '未提供';
    const contact = route.query.contact || '未提供';
    
    const p2 = route.query.p2;
    const p3 = route.query.p3;
    const p4 =route.query.p4;
    const p5 =route.query.p5;
    const translatedp2 = computed(() => t(p2 as string|| 'defaultKey'));
    const translatedp3 = computed(() => t(p3 as string|| 'defaultKey'));
    const translatedp4 = computed(() => t(p4 as string|| 'defaultKey'));
    const translatedp5 = computed(() => t(p5 as string|| 'defaultKey'));

    const DateArrivalShipReturn = route.query.DateArrivalShipReturn || '未提供';
    const DateShuttleReturn = route.query.DateShuttleReturn || '未提供';
    const TimeArrivalShipReturn = route.query.TimeArrivalShipReturn || '未提供';
    const TimeShuttleReturn = route.query.TimeShuttleReturn || '未提供';
    
    return {
        t,
      tab,
      adult,
      child,
      totalPrice,
      DateArrivalShip,
      DateShuttle,
      TimeArrivalShip,
      TimeShuttle,
      phone,
      contact,
      translatedp2,
      translatedp3,
      translatedp4,
      translatedp5,
      localPath,
      TimeArrivalShipReturn,
      DateArrivalShipReturn,
      DateShuttleReturn,
      TimeShuttleReturn,
    };
  },
});
</script>
