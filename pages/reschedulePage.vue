<template>
  <header class="py-4 border-b bg-green-400 flex items-center">
    <div>
      <BackBtn />
    </div>
    <h1 class="absolute left-1/2 transform -translate-x-1/2 text-xl font-bold capitalize">
      {{ $t('ReschedulePage.title') }}
    </h1>
  </header>

  <div class="text-red-600 text-sm flex bg-yellow-300 pl-4">
    {{ $t('ReschedulePage.message') }}
  </div>

  <div class="px-6 pt-4">
    <div class="bg-white p-4 rounded-lg flex flex-col border-b-2 ">
      <div class="font-bold">
        <p>{{ $t('ReschedulePage.originalSchedule') }}</p>
      </div>
      <div class="text-gray-500 ">
        <div class="flex">
              <div class="flex-1 self-center"><p class="text-end">{{ $t('bookingStatus.pier') }}</p></div>
              <div class="flex-1 flex-col text-center">
                <div class="border-b-2 border-dashed mx-4 border-green-400 text-sm">
                  <p>{{ $t('orderDetail.date') }}</p>
                </div>
                <div class="text-sm">
                  <p>{{ $t('orderDetail.time') }}</p>
                </div>
              </div>
              <div class="flex-1 self-center"><p>{{ $t('bookingStatus.airport') }}</p></div>
        </div>
          <div class="grid grid-cols-2 pt-4 pl-6">
            <div class="flex-col justify-self-end">
              <p>{{ $t('Booking.flightNumber') }} :  MF1544</p>
              <p>{{ $t('Booking.contact') }} :  XXX</p>
              <p>{{ $t('Booking.phone') }} :  183XXXXXX</p>
                <p>
                  {{ $t('bookingStatus.adultTicket') }}
                </p>
                <p >
                  {{ $t('bookingStatus.infantTicket') }}
                </p>
            </div>
          </div>
      </div>
    </div>
      </div>
      <div class="px-6 pb-4">
        <div class="bg-white pt-4 pr-4 pl-4 rounded-t-lg flex flex-col text-sm ">
      <!-- 重新安排 -->
      <div class="font-bold">
        <p>{{ $t('ReschedulePage.newSchedule') }}</p>
      </div>
      <div class="flex text-yellow-600">
        <p v-if="isSwapped" class="underline">{{ $t('Booking.airport') }}</p>
        <p v-else>{{ $t('Booking.pier') }}</p>

        <Icon
          name="i-material-symbols:360"
          class="mx-4 bg-green-600 w-5 h-5 cursor-pointer"
          @click="swapText"
        />

        <p v-if="isSwapped" class="underline">{{ $t('Booking.pier') }}</p>
        <p v-else>{{ $t('Booking.airport') }}</p>
      </div>

      <!-- 出发时间 -->
      <div class="font-bold pt-2 text-yellow-600">
        <p>{{ $t('ReschedulePage.departureTime') }}</p>
      </div>
      <a-config-provider :locale="antLocale">
        <div class="flex space-x-4">
          <a-space direction="vertical" :size="12">
            <a-date-picker 
            :show-today="false"
            :disabled-date="disabledDate"
            />
          </a-space>
          <a-time-picker
            :minute-step="isSwapped ? 5 : 30"
            value-format="HH:mm"
            format="HH:mm"
            :disabled-hours="disabledHours"
            :disabled-minutes="disabledMinutes"
            :hide-disabled-options="true" 
            :show-now="false"       
          ></a-time-picker>
        </div>
      </a-config-provider>
              <div class="flex flex-col py-2 ">
                <div class="flex items-center  ">
                  <div class="flex pr-4 ">
                    <Icon name="lucide:tickets" class="bg-yellow-600 w-5 h-5" />
                    <p class="text-yellow-600 ">
                      {{ isSwapped ? $t('Booking.ferryTime') : $t('Booking.flightNumber') }}
                    </p>
                  </div>
                  <div>
                    <template v-if="isSwapped">
                      <a-config-provider :locale="antLocale">
                        <a-time-picker
                          v-model:value="ferryTime"
                          :minute-step="30"
                          value-format="HH:mm"
                          format="HH:mm"
                          :disabled-hours="disabledHours"
                          :disabled-minutes="disabledMinutes"
                          :hide-disabled-options="true"
                          :show-now="false"
                        ></a-time-picker>
                      </a-config-provider>
                    </template>
                    <template v-else>
                      <div class="border rounded-lg">
                        <input
                          type="text"
                          :placeholder="$t('Booking.required')"
                          v-model="flightNumber"
                          class="flex-1 px-2 py-1 w-full bg-white rounded-lg focus:outline-none text-sm"
                        />
                      </div>
                    </template>
                  </div>
                </div>
              </div>
      <div class="font-bold pt-2 text-yellow-600">
        <p>{{ $t('ReschedulePage.passengerCount') }}</p>
      </div>
        <div class="flex items-center ">
          <p class="pr-24 ">{{ $t('Booking.adultTicket') }}</p>
          <div class="flex items-center space-x-2">
            <button @click="decrement('adult')">
              <Icon name="i-mdi-minus-circle-outline"/>
            </button>
            <p class="px-2 text-sm text-yellow-600">{{ counts.adult }}</p>
            <button @click="increment('adult')">
              <Icon name="i-material-symbols-add-circle-outline"/>
            </button>
          </div>
        </div>
  
        <!-- 儿童票 -->
        <div class="flex items-center pt-2">
          <p class="pr-10">{{ $t('Booking.childTicket') }}</p>
          <div class="flex items-center space-x-2">
            <button @click="decrement('child')">
              <Icon name="i-mdi-minus-circle-outline"/>
            </button>
            <p class="px-2 text-sm text-yellow-600">{{ counts.child }}</p>
            <button @click="increment('child')">
              <Icon name="i-material-symbols-add-circle-outline"/>
            </button>
          </div>
        </div>
        <div class="flex-col">
          <div class="flex pt-2">
                <p class="flex-none pr-6">{{ $t('Booking.contact') }}</p>
                <input
                  type="text"
                  :placeholder="$t('Booking.required')"
                  v-model="contact"
                  class="flex-1 px-4 border-b-2 border-gray-600 bg-inherit focus:outline-none text-sm min-w-0"/>
              </div>

              <div class="flex py-4 content-center">
                <p class="flex-none pr-2">{{ $t('Booking.phone') }}</p>
                <input
                  type="text"
                  :placeholder="$t('Booking.required')"
                  v-model="phone"
                  class="flex-1 px-4 border-b-2 border-gray-600 bg-inherit focus:outline-none text-sm min-w-0"/>
              </div>
        </div>
        <!-- 票价 -->
        <div class="flex items-center pt-3">
          <p class="pr-28"> {{ $t('ReschedulePage.amountDue') }}</p>
          <p class="text-red-500">&yen; {{ totalPrice }}.00</p>
        </div>
         
      </div>
      <div class="flex bg-white pt-6 rounded-b-lg mb-12">
        <div class="flex-1 border-r-2 border-t-2 rounded">
          <NuxtLink class=" text-blue-400 py-2 rounded flex justify-center">{{ $t('ReschedulePage.confirmReschedule') }}</NuxtLink>
        </div>
        <div class="flex-1 border-t-2 rounded">
          <NuxtLink class=" text-red-500 py-2 rounded flex justify-center">{{ $t('cancelOrder') }}</NuxtLink>
        </div>
      </div>

  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';
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
    const counts = ref({ adult: 1, child: 1 });
    const pricePerTicket = ref(30);
    const phone = ref(''); // 聯繫電話
    const contact = ref('');

    const totalPrice = computed(() => {
      return counts.value.adult  * pricePerTicket.value;
    });

    const { locale } = useI18n();
    const isSwapped = ref(false);
    const swapText = () => {
      isSwapped.value = !isSwapped.value;
    };
    const flightNumber = ref('');
    const ferryTime = ref<string>('');
    const antLocales = {
      'zh-CN': zhCN,
      'zh-TW': zhTW,
      'en-US': enUS,
    };

    const antLocale = computed(() => antLocales[locale.value] || zhTW);
    const disabledDate = (current: Dayjs): boolean => {
       return current && current.isBefore(dayjs().add(1, 'day').startOf('day'))
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
        return Array.from({ length: 60 }, (_, i) => i).filter(minute => minute >= 1); // 17:30 之后禁用
    }
    return [];
    };


    return {
      isSwapped,
      swapText,
      antLocale,
      counts,
      totalPrice,
      phone,
      contact,
      disabledDate,
      disabledHours,
      disabledMinutes,
      flightNumber,
      ferryTime,
      increment(type: 'adult' | 'child') {
        counts.value[type]++;
      },
      decrement(type: 'adult' | 'child') {
        if (counts.value[type] > 0) {
          counts.value[type]--;
        }
      },
    };
  },
});
</script>
