<template>
    <div class="flex-col p-4">
        <div>
            <p class="border-l-4 my-3 border-orange-400 text-justify pl-2">{{ $t('Booking.outward') }}</p>
        </div>
        <div class="flex justify-around">
                <div class="flex"><Icon name="i-material-symbols-directions-car" class="bg-gray-500 w-6 h-6"/>
                  <p class="flex-1 text-gray-500 ml-2">{{ $t('Booking.departure') }}</p>
                </div>
                <div class="flex"><Icon name="i-mdi-map-marker-radius-outline" class="bg-gray-500 w-6 h-6"/>
                  <p class="flex-1 text-gray-500 ml-2">{{ $t('Booking.arrival') }}</p>
                </div>
              </div>
              <div class="flex justify-evenly">
                <p class="ml-auto text-2xl ">{{ isSwapped ? returnTrip.p2 : onwardTrip.p2 }}</p>
                <Icon name="i-hugeicons-exchange-01" class=" mx-auto bg-green-600 w-7 h-7"
                @click="swapText"/>
                <p class="mr-auto text-2xl">{{ isSwapped ? returnTrip.p3 : onwardTrip.p3 }}</p>
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
                      :disabled-date="disabledDateAfter"
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
              <div class="border-b-2 py-3"></div>

                <p class="border-l-4 my-3 border-orange-400 text-justify pl-2">{{ $t('Booking.return') }}</p>
                <div class="flex justify-around">
                <div class="flex"><Icon name="i-material-symbols-directions-car" class="bg-gray-500 w-6 h-6"/>
                  <p class="flex-1 text-gray-500 ml-2">{{ $t('Booking.departure') }}</p>
                </div>
                <div class="flex"><Icon name="i-mdi-map-marker-radius-outline" class="bg-gray-500 w-6 h-6"/>
                  <p class="flex-1 text-gray-500 ml-2">{{ $t('Booking.arrival') }}</p>
                </div>
              </div>
              <div class="flex justify-evenly">
                <p class="ml-auto text-2xl ">{{ isSwapped ? onwardTrip.p2 : returnTrip.p2 }}</p>
                <Icon name="i-hugeicons-exchange-01" class=" mx-auto bg-green-600 w-7 h-7"
                @click="swapText"/>
                <p class="mr-auto text-2xl"> {{ isSwapped ? onwardTrip.p3 : returnTrip.p3 }}</p>
              </div>
              <div class="border-b-2 py-3"></div>
              <div class="flex pt-3 pl-4">
                <Icon name="i-majesticons-ship-line" class="bg-green-300 w-6 h-6"></Icon>
                <p class="text-green-300">{{ $t('Booking.arrivalPortTime') }}</p>
              </div>
              <a-config-provider :locale="antLocale">
                <div class="flex pt-2 pl-4 space-x-4">
                  <a-space direction="vertical" :size="12">
                    <a-date-picker v-model:value="DateArrivalShipReturn" :disabled-date="disabledDate" />
                  </a-space>
                  <a-time-picker
                    v-model:value="TimeArrivalShipReturn"
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
                      v-model:value="DateShuttleReturn" 
                      :disabled="!DateArrivalShipReturn" 
                      :disabled-date="disabledDateReturn"
                    />
                  </a-space>
                  <a-time-picker
                    v-model:value="TimeShuttleReturn"
                    value-format="HH:mm"
                    :disabled="!TimeArrivalShipReturn"
                    format="HH:mm"
                  ></a-time-picker>
                </div>
              </a-config-provider>
              <DashLine/>
              <div class="flex pl-6 pt-2 justify-evenly content-center">
                <div class="flex flex-1">
                  <p class="flex-1">{{ $t('Booking.adultTicket') }}</p>
                  <div class="flex-1 flex ">
                  <button @click="decrement('adult')">
                  <Icon name="i-mdi-minus-circle-outline"/></button>
                  <p class="px-2">{{ counts.adult }}</p>
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
                  v-model="contact"
                  type="text"
                  placeholder="必填"
                  class="flex-1 px-2 border-b-2 border-gray-600 bg-inherit focus:outline-none text-sm min-w-0"/>
              </div>

              <div class="flex py-4 px-6 content-center">
                <p class="flex-none pr-2">{{ $t('Booking.phone') }}</p>
                <input
                  v-model="phone"
                  type="text"
                  placeholder="必填"
                  class="flex-1 px-2 border-b-2 border-gray-600 bg-inherit focus:outline-none text-sm min-w-0"/>
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
    // 狀態
    const tab = ref<string>('roundedway');
    const { t } = useI18n();
    const localPath = useLocalePath();
    const { locale } = useI18n(); 
    const isSwapped = ref(false);
    const DateArrivalShip = ref<Dayjs>();
    const DateShuttle = ref<Dayjs>(); // 選擇的日期
    const DateArrivalShipReturn = ref<Dayjs>();
    const DateShuttleReturn = ref<Dayjs>();
    const TimeArrivalShip = ref<string>('');
    const TimeShuttle = ref<string>('');
    const TimeArrivalShipReturn = ref<string>('');
    const TimeShuttleReturn = ref<string>('');
    const isChecked = ref(false); // 定義復選框狀態
    const counts = ref({
      adult: 1,
      child: 1,
    });
    const pricePerTicket = ref(60);
    const contact = ref("");
    const phone = ref("");
    const totalPrice = computed(() => {
      return counts.value.adult * pricePerTicket.value;
    });

    const onwardTrip = ref({
      p2: t('Booking.pier'),
      p3: t('Booking.airport'),
    });
    const returnTrip = ref({
      p2: t('Booking.airport'),
      p3: t('Booking.pier'),
    });

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

    watch(TimeArrivalShipReturn, (newTimeShuttleReturn) => {
      if (newTimeShuttleReturn) {
        // 將第一個時間加上 45 分鐘
        TimeShuttleReturn.value = dayjs(newTimeShuttleReturn, 'HH:mm')
          .subtract(45, 'minute')
          .format('HH:mm');
      } else {
        // 如果第一個時間清空，第二個時間框也重置
        TimeShuttleReturn.value = '';
      }
    });

    watch(DateArrivalShipReturn, (newValue: Dayjs | undefined) => {
      if (newValue) {
        DateShuttleReturn.value = newValue; // 初始化返回班车时间
      } else {
        DateShuttleReturn.value = undefined; // 如果清空返回船只时间，也清空班车时间
      }
    });
    watch(DateArrivalShip, (newValue: Dayjs | undefined) => {
      if (newValue) {
        DateShuttle.value = newValue; // 初始化返回班车时间
      } else {
        DateShuttle.value = undefined; // 如果清空返回船只时间，也清空班车时间
      }
    });

    const disabledDate = (current: Dayjs): boolean => {
    return current && current.isBefore(dayjs().startOf('day'));
    };

    const disabledDateAfter = (current: Dayjs): boolean => {
      return current && (!DateArrivalShip.value || current.isBefore(DateArrivalShip.value));
    };

    const disabledDateReturn = (current: Dayjs): boolean => {
      return current && (!DateArrivalShip.value || current.isBefore(DateArrivalShip.value));
    };
    // 方法
    const swapText = () => {
      isSwapped.value = !isSwapped.value;
    };
    const antLocales = {
      'zh-CN': zhCN,
      'zh-TW': zhTW,
      'en-US': enUS,
    };
    const antLocale = computed(() => antLocales[locale.value] || zhTW);
  
    const openNotification = (placement: NotificationPlacement) => {
      notification.open({
        message: t('HomePage.notification.title'),
        description: t('HomePage.notification.description'),
        placement,
      });
    };

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
            phone: phone.value,
            contact: contact.value,
            totalPrice: totalPrice.value,
            DateArrivalShip: DateArrivalShip.value? DateArrivalShip.value.format("YYYY-MM-DD") : undefined,
            DateShuttle: DateShuttle.value? DateShuttle.value.format("YYYY-MM-DD") : undefined,
            TimeArrivalShip: TimeArrivalShip.value,
            TimeShuttle: TimeShuttle.value,
            TimeArrivalShipReturn:TimeArrivalShipReturn.value,
            DateArrivalShipReturn:DateArrivalShipReturn.value?DateArrivalShipReturn.value.format("YYYY-MM-DD") : undefined,
            DateShuttleReturn:DateShuttleReturn.value?DateShuttleReturn.value.format("YYYY-MM-DD") : undefined,
            TimeShuttleReturn:TimeShuttleReturn.value,
            p2:isSwapped.value ? returnTrip.value.p2 : onwardTrip.value.p2, 
            p3:isSwapped.value ? returnTrip.value.p3 : onwardTrip.value.p3,
            p4:isSwapped.value ? onwardTrip.value.p2 : returnTrip.value.p2,
            p5:isSwapped.value ? onwardTrip.value.p3 : returnTrip.value.p3,
          },
        });
      }
    };

    return {
      t,
      tab,
      isSwapped,
      antLocale,
      onwardTrip,
      returnTrip,
      counts,
      contact,
      phone,
      totalPrice,
      swapText,
      isChecked,
      navigateToConfirmation,
      disabledHours,
      disabledMinutes,
      TimeArrivalShip,
      TimeShuttle,
      DateArrivalShip,
      DateShuttle,
      TimeArrivalShipReturn,
      DateArrivalShipReturn,
      DateShuttleReturn,
      TimeShuttleReturn,
      disabledDate,
      disabledDateAfter, 
      disabledDateReturn,
      increment(type: 'adult' | 'child') {
        counts.value[type]++;
      },
      decrement(type: 'adult' | 'child') {
        if (counts.value[type] > 0) {
          counts.value[type]--;
        }
      },
      pricePerTicket: 60,
    };
    
  },
});
</script>
