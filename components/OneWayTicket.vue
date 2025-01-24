<template>
    <div class="flex-col p-4">
        <div class="flex justify-around">
                <div class="flex"><Icon name="i-material-symbols-directions-car" class="bg-gray-500 w-6 h-6"/>
                  <p class="flex-1 text-gray-500 ml-2">{{ $t('Booking.departure') }}</p>
                </div>
                <div class="flex"><Icon name="i-mdi-map-marker-radius-outline" class="bg-gray-500 w-6 h-6"/>
                  <p class="flex-1 text-gray-500 ml-2">{{ $t('Booking.arrival') }}</p>
                </div>
              </div>
              <div class="flex justify-evenly">
                  <!-- 根據狀態決定順序 -->
                  <p v-if="isSwapped" class="ml-auto text-2xl">{{ $t('Booking.airport') }}</p>
                  <p v-else class="ml-auto text-2xl">{{ $t('Booking.pier') }}</p>
                  
                  <Icon
                    name="i-hugeicons-exchange-01"
                    class="mx-auto bg-green-600 w-7 h-7 cursor-pointer"
                    @click="swapText"
                  />

                  <p v-if="isSwapped" class="mr-auto text-2xl">{{ $t('Booking.pier') }}</p>
                  <p v-else class="mr-auto text-2xl">{{ $t('Booking.airport') }}</p>
                </div>
              <div class="border-b-2 py-3"></div>
              <div class="flex pt-3 pl-4">
                <Icon name="i-majesticons-ship-line" class="bg-green-300 w-6 h-6"></Icon>
                <p class="text-green-300">{{ $t('Booking.arrivalPortTime') }}</p>
              </div>
              <a-config-provider :locale="antLocale">
                <div class="flex pt-2 pl-4 space-x-4">
                  <a-space direction="vertical" :size="12">
                    <a-date-picker v-model:value="DateArrivalShip" :disabled-date="disabledDate" />
                  </a-space>
                  <a-time-picker
                    v-model:value="TimeArrivalShip"
                    value-format="HH:mm"
                    :disabled-hours="disabledHours"
                    :disabled-minutes="disabledMinutes"
                    format="HH:mm"
                  ></a-time-picker>
                </div>
              </a-config-provider>

              <div class="flex pt-3 pl-4">
                <Icon name="i-material-symbols-directions-car" class="bg-green-300 w-6 h-6"></Icon>
                <p class="text-green-300">{{ $t('Booking.shuttleBusTime') }}</p>
              </div>
              <a-config-provider :locale="antLocale">
                <div class="flex pt-2 pl-4 space-x-4">
                  <a-space direction="vertical" :size="12">
                    <a-date-picker 
                      v-model:value="DateShuttle" 
                      :disabled="!DateArrivalShip" 
                      :disabled-date="disabledDateAfterFirst"
                    />
                  </a-space>
                  <a-time-picker
                    v-model:value="TimeShuttle"
                    value-format="HH:mm"
                    :disabled="!TimeArrivalShip"
                    format="HH:mm"
                  ></a-time-picker>
                </div>
              </a-config-provider>
              <DashLine/>
              <div class="flex pl-6 pt-2 justify-evenly content-center">
                <div class="flex flex-1">
                  <p class="flex-1">{{ $t('Booking.adultTicket') }}</p>
                  <div class="flex-1 flex  ">
                  <button @click="decrement('adult')">
                  <Icon name="i-mdi-minus-circle-outline"/></button>
                  <p class="px-2 flex">{{ counts.adult }}</p>
                  <button @click="increment('adult')">
                    <Icon name="i-material-symbols-add-circle-outline"/>
                  </button>
                  </div>
                </div>
                <div class="flex flex-1">
                  <p class="flex-1">{{ $t('Booking.childTicket') }}</p>
                  <div class="flex-1 flex">
                  <button @click="decrement('child')">
                  <Icon name="i-mdi-minus-circle-outline"/></button>
                  <p class="px-2 ">{{ counts.child }}</p>
                  <button @click="increment('child')">
                    <Icon name="i-material-symbols-add-circle-outline"/>
                  </button>
                  </div>
                </div>
              </div>
              <div class="flex pl-6 pt-3">
                <p class="flex-1">{{ $t('Booking.ticketPrice') }}</p>
                <p class="flex-1">&yen; {{ totalPrice }}{{ $t('Booking.pricePerPerson') }}</p>
                <p class="flex-1"></p>
              </div>


              <div class="flex py-4 px-6 content-center">
                <p class="flex-none pr-6">{{ $t('Booking.contact') }}</p>
                <input
                  type="text"
                  placeholder="必填"
                  v-model="contact"
                  class="flex-1 px-2 border-b-2 border-gray-600 bg-inherit focus:outline-none text-sm min-w-0"/>
              </div>

              <div class="flex py-4 px-6 content-center">
                <p class="flex-none pr-2">{{ $t('Booking.phone') }}</p>
                <input
                  type="text"
                  placeholder="必填"
                  v-model="phone"
                  class="flex-1 px-2 border-b-2 border-gray-600 bg-inherit focus:outline-none text-sm min-w-0"/>
              </div>
              
              
            </div>
            <div class="flex justify-center">
                <input
                    type="checkbox"
                    id="checkbox"
                    v-model="isChecked"
                    class="h-5 w-3 border-black rounded-full"/>
                    <label for="checkbox" class="ml-2 text-sm text-gray-700">{{ $t('HomePage.readAgreement') }}</label>
                    <PopUpRules/>
              </div>
            <div class="pt-1 flex justify-center pb-4">
                <a-button
                  type="primary"
                  @click="navigateToConfirmation"
                 class="w-2/3 rounded-lg bg-green-500 text-white">{{ $t('HomePage.confirm') }}</a-button>
              </div>
</template>

<script lang="ts">
import { defineComponent,ref,watch } from 'vue';
import type { NotificationPlacement,ConfigProvider } from 'ant-design-vue';
import { useI18n } from 'vue-i18n';
import type { Dayjs } from 'dayjs';
import dayjs from 'dayjs';
import zhCN from 'ant-design-vue/es/locale/zh_CN';
import zhTW from 'ant-design-vue/es/locale/zh_TW';
import enUS from 'ant-design-vue/es/locale/en_US';

export default defineComponent({
    setup() {
    const { locale } = useI18n(); 
    const localPath = useLocalePath();
    const isChecked = ref(false); // 定義復選框狀態
    const DateArrivalShip = ref<Dayjs>();
    const DateShuttle = ref<Dayjs>(); // 選擇的日期
    const TimeArrivalShip = ref<string>('');
    const TimeShuttle = ref<string>(''); // 選擇的時間
    const phone = ref(''); // 聯繫電話
    const contact = ref(''); // 聯繫人
    const counts = ref({ adult: 1, child: 1 }); // 成人和兒童票數
    const pricePerTicket = ref(30); // 單張票價
    const tab = ref<string>('oneway');
    const totalPrice = computed(() => {
      return counts.value.adult * pricePerTicket.value;
    });
    const isSwapped = ref(false);
    
    const antLocales = {
      'zh-CN': zhCN,
      'zh-TW': zhTW,
      'en-US': enUS,
    };
    const antLocale = computed(() => antLocales[locale.value] || zhTW);
    const { t } = useI18n();

    // 打开通知

    const openNotification = (placement: NotificationPlacement) => {
      notification.open({
        message: t('HomePage.notification.title') ,
        description: t('HomePage.notification.description'),
        placement,
      });
    };
    
    const now = dayjs();
    const disabledHours = () => {
      const currentHour = now.hour();
      return Array.from({ length: 24 }, (_, i) => i).filter(hour => hour < currentHour);
    };
    const disabledMinutes = (selectedHour: number | null) => {
      const currentHour = now.hour();
      const currentMinute = now.minute();

      // 如果未選擇小時，禁用所有分鐘選項
      if (selectedHour === null || selectedHour === undefined) {
        return Array.from({ length: 60 }, (_, i) => i);
      }

      // 如果選中的小時是當前小時，禁用早於當前分鐘的選項
      if (selectedHour === currentHour) {
        return Array.from({ length: 60 }, (_, i) => i).filter(minute => minute < currentMinute);
      }

      // 其他情況，不禁用任何分鐘
      return [];
    };
    
    watch(TimeArrivalShip, (newTimeShuttle) => {
      if (newTimeShuttle) {
        // 將第一個時間加上 45 分鐘
        TimeShuttle.value = dayjs(newTimeShuttle, 'HH:mm')
          .add(45, 'minute')
          .format('HH:mm');
      } else {
        // 如果第一個時間清空，第二個時間框也重置
        TimeShuttle.value = '';
      }
    });
    // 點擊確認按鈕的邏輯
    const navigateToConfirmation = () => {
      
      if (!isChecked.value) {
        openNotification('bottom'); // 如果未勾選，彈出通知
      } else {
        const path = localPath('/confirmationPage');
        navigateTo({
          path,
          query: {
            tab: tab.value,
            adult: counts.value.adult,
            child: counts.value.child,
            totalPrice: totalPrice.value,
            DateArrivalShip: DateArrivalShip.value? DateArrivalShip.value.format("YYYY-MM-DD") : undefined,
            DateShuttle: DateShuttle.value? DateShuttle.value.format("YYYY-MM-DD") : undefined,
            TimeArrivalShip: TimeArrivalShip.value,
            TimeShuttle: TimeShuttle.value,
            phone: phone.value,
            contact: contact.value,
            p2: isSwapped.value ? 'Booking.airport' : 'Booking.pier',
            p3: isSwapped.value ? 'Booking.pier' : 'Booking.airport',
          },
        });
      }
    };

    const swapText = () => {
    isSwapped.value = !isSwapped.value;
    };
    
    const disabledDate = (current: Dayjs): boolean => {
    return current && current.isBefore(dayjs().startOf('day'));
    };

    const disabledDateAfterFirst = (current: Dayjs): boolean => {
      return current && (!DateArrivalShip.value || current.isBefore(DateArrivalShip.value));
    };

    return {
      t,
      tab,
      antLocale,
      isChecked,
      navigateToConfirmation,
      DateArrivalShip,
      DateShuttle,
      TimeArrivalShip,
      TimeShuttle,
      phone,
      contact,
      counts,
      totalPrice,
      disabledHours,
      disabledMinutes,
      isSwapped,
      swapText,
      increment(type: 'adult' | 'child') {
        counts.value[type]++;
      },
      decrement(type: 'adult' | 'child') {
        if (counts.value[type] > 0) {
          counts.value[type]--;
        }
      },
      pricePerTicket: 30,
      disabledDate,
      disabledDateAfterFirst, 
    };
  },
  watch: {
      DateArrivalShip(newValue: Dayjs | undefined) {
      if (newValue) {
        this.DateShuttle = newValue; 
      } else {
        this.DateShuttle = undefined; 
      }
    },
  },
});
</script>
