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
                <Icon :name="isSwapped ? 'i-material-symbols-flight-land' : 'i-majesticons-ship-line'" 
                 class="bg-green-300 w-6 h-6"></Icon>
                <p v-if="isSwapped" class="text-green-300">{{ $t('Booking.arrivalAtAirportTime') }}</p>
                <p v-else class="text-green-300">{{ $t('Booking.arrivalPortTime') }}</p>
              </div>
              <a-config-provider :locale="antLocale">
                <div class="flex pt-2 pl-4 space-x-4">
                  <a-space direction="vertical" :size="12">
                    <a-date-picker 
                    v-model:value="DateArrivalShip" 
                    :disabled-date="disabledDate"
                    :show-today="false"
                    @open-change="handleDatePickerOpen"
                    />
                  </a-space>
                  <a-time-picker
                    v-model:value="TimeArrivalShip"
                    value-format="HH:mm"
                    :minute-step="30"
                    :hour-step="1"
                    :disabled-hours="disabledHours"
                    :disabled-minutes="disabledMinutes"
                    :hide-disabled-options="true" 
                    :show-now="false"
                    format="HH:mm"
                  ></a-time-picker>
                </div>
              </a-config-provider>
              <div class="flex flex-col py-2 pl-4 ">
                <div class="flex items-center">
                  <div class="flex pr-4">
                    <Icon name="lucide:tickets"  class="bg-green-300 w-6 h-6"/>
                    <p class="text-green-300">{{ $t('Booking.flightNumber') }} </p>
                  </div>
                  <div class="border rounded-lg w-2/5">
                    <input
                    type="text"
                    :placeholder="$t('Booking.required')"
                    v-model="flightNumber"
                    class="flex-1 px-2 py-1 w-full bg-white rounded-lg focus:outline-none text-sm"/>
                  </div>
                </div>
              </div>

              <div class="flex pl-4">
                <Icon name="i-material-symbols-directions-car" class="bg-green-300 w-6 h-6"></Icon>
                <p class="text-green-300">{{ $t('Booking.shuttleBusTime') }}</p>
              </div>
              <a-config-provider :locale="antLocale">
                <div class="flex pt-2 pl-4 space-x-4">
                  <a-space direction="vertical" :size="12">
                    <a-date-picker 
                      v-model:value="DateShuttle" 
                      :disabled="true" 
                      :disabled-date="disabledDateAfterFirst"
                    />
                  </a-space>
                  <a-time-picker
                    v-model:value="TimeShuttle"
                    :minute-step="30"
                    value-format="HH:mm"
                    :disabled="true"
                    format="HH:mm"
                  ></a-time-picker>
                </div>
              </a-config-provider>
              <DashLine/>
              <div class="flex pl-6 pt-2 justify-evenly content-center">
                <div class="flex flex-1 ">
                  <p class="flex-1">{{ $t('Booking.adultTicket') }}</p>
                  <div class="flex-1 flex  ">
                  <button @click="decrement('adult')">
                  <Icon name="i-mdi-minus-circle-outline"/></button>
                  <p class="px-2 flex text-sm">{{ counts.adult }}</p>
                  <button @click="increment('adult')">
                    <Icon name="i-material-symbols-add-circle-outline"/>
                  </button>
                  </div>
                </div>
              </div>
              <div class="flex pl-6 pt-2 justify-evenly content-center">
                <div class="flex flex-1 ">
                  <p class="flex-1">{{ $t('Booking.childTicket') }}</p>
                  <div class="flex-1 flex ">
                  <button @click="decrement('child')">
                  <Icon name="i-mdi-minus-circle-outline"/></button>
                  <p class="px-2 text-sm">{{ counts.child }}</p>
                  <button @click="increment('child')">
                    <Icon name="i-material-symbols-add-circle-outline"/>
                  </button>
                  </div>
                </div>
              </div>
              <div class="flex pl-6 pt-3 justify-evenly content-center">
                <p class="flex-1">{{ $t('Booking.ticketPrice') }}</p>
                <p class="flex-1">&yen; {{ totalPrice }}{{ $t('Booking.pricePerPerson') }}</p>
              </div>


              <div class="flex py-4 px-6 content-center">
                <p class="flex-none pr-6">{{ $t('Booking.contact') }}</p>
                <input
                  type="text"
                  :placeholder="$t('Booking.required')"
                  v-model="contact"
                  class="flex-1 px-2 border-b-2 border-gray-600 bg-inherit focus:outline-none text-sm min-w-0"/>
              </div>

              <div class="flex py-4 px-6 content-center">
                <p class="flex-none pr-2">{{ $t('Booking.phone') }}</p>
                <input
                  type="text"
                  :placeholder="$t('Booking.required')"
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
import type { NotificationPlacement} from 'ant-design-vue';
import { Icon } from '@iconify/vue'
import { useI18n } from 'vue-i18n';
import type { Dayjs } from 'dayjs';
import dayjs from 'dayjs';
import 'dayjs/locale/zh-cn';
import 'dayjs/locale/zh-tw';
import 'dayjs/locale/en';
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
    const phone = ref('');
    const flightNumber = ref('');
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

    const disabledHours = () => {
  // 只允许 8 到 17 点
     return Array.from({ length: 24 }, (_, i) => i).filter(hour => hour < 8 || hour > 18);
    };

    const disabledMinutes = (selectedHour: number | null) => {
    if (selectedHour === 8) {
        return Array.from({ length: 60 }, (_, i) => i).filter(minute => minute < 30); // 8:30 之前禁用
    }
    if (selectedHour === 18) {
        return Array.from({ length: 60 }, (_, i) => i).filter(minute => minute >= 30); // 17:30 之后禁用
    }
    return [];
    };
    
    watch(TimeArrivalShip, (newTimeShuttle) => {
      if (!DateArrivalShip.value) {
    // 如果 DateArrivalShip 沒有選擇，清空 TimeArrivalShip 並提示
        TimeArrivalShip.value = '';
        alert(t('alertMessage'));
        return;
      }
      if (newTimeShuttle) {
        const newShuttleTime = dayjs(newTimeShuttle, 'HH:mm').add(5, 'minute');

        // 如果時間超過午夜，DateShuttle加一天
        if (newShuttleTime.isAfter(dayjs(newTimeShuttle, 'HH:mm').endOf('day'))) {
          DateShuttle.value = DateArrivalShip.value?.add(1, 'day');
        } else {
          DateShuttle.value = DateArrivalShip.value;
        }

        TimeShuttle.value = newShuttleTime.format('HH:mm');
      } else {
        // 如果第一個時間清空，第二個時間框也重置
        TimeShuttle.value = '';
      }
    });

    const handleDatePickerOpen = (open : boolean) => {
    if (open) {
    message.info(t('alertMessage2'));
    }
    };
    watch(DateArrivalShip, (newValue: Dayjs | undefined) => {
      if (newValue) {
        DateShuttle.value = newValue; // 初始化返回班車時間

        // 重置 TimeArrivalShip 的值，防止顯示默認時間
        TimeArrivalShip.value = '';
      } else {
        DateShuttle.value = undefined; // 如果清空返回船只時間，也清空班車時間
        TimeArrivalShip.value = ''; // 清空時間選擇器
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
            flightNumber:flightNumber.value,
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
       return current && current.isBefore(dayjs().add(1, 'day').startOf('day'))
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
      flightNumber,
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
      handleDatePickerOpen
    };
  },
});
</script>
