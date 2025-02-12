<template>
    <header class="py-4 border-b bg-green-400 flex items-center">
      <div class="flex">
        <BackBtn/>
      </div>
      <h1 class="absolute left-1/2 transform -translate-x-1/2 text-2xl font-bold capitalize">
        {{ $t('profileSettings.passenger') }}
      </h1>
    </header>
  
    <div>  
      <div class="px-6 py-4">
        <p class="text-2xl pb-2 font-bold">{{ $t('PassengerPage.passengers') }}</p>
        <div class="bg-white p-4 rounded-lg flex mb-4">
            <div class="flex text-normal pl-2 flex-1">
            <div class="flex flex-1 flex-col">
              <div class="flex flex-1">
                <div class="bg-green-200 py-1 px-2 text-green-900 rounded-sm">
                 <p>{{ $t('PassengerPage.passengers1') }}</p>
                </div>
              </div>
              <div> 
              </div>
              <div class="pt-4 mx-4">
                <div class="flex">
                  <p class="font-bold">{{ $t('PassengerPage.passengers1') }}</p>
                  <div class="pl-2 rounded-lg">
                    <p class="text-xs bg-gray-300 px-2">{{ $t('PassengerPage.local') }}</p>
                  </div>
                </div>
                <div class="text-gray-400">
                  <div class="flex">
                    <p>{{ $t('PassengerPage.IdNumber') }} : </p>
                    <p class="pl-2">9314XXXXX0932</p>
                  </div>
                  <div class="flex">
                    <p>{{ $t('PassengerPage.phoneNumber') }} : </p>
                    <p class="pl-2">18XXX2938</p>
                  </div>
                  <div class="flex">
                    <p>{{ $t('PassengerPage.birthday') }} : </p>
                    <p class="pl-2">2000-01-01</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="place-content-center">
            <img src="/assets/icons/GreaterThan.png" alt="More" class="w-6 h-6"/>
          </div>
        </div>
        <div class="bg-white p-4 rounded-lg flex mb-4">
            <div class="flex text-normal pl-2 flex-1">
            <div class="flex flex-1 flex-col">
              <div class="flex flex-1">
                <div class="bg-red-400 py-1 px-2 text-white rounded-sm">
                 <p>{{ $t('PassengerPage.passengers2') }}</p>
                </div>
              </div>
              <div> 
              </div>
              <div class="pt-4 mx-4">
                <div class="flex">
                  <p class="font-bold">{{ $t('PassengerPage.passengers2') }}</p>
                  <div class="pl-2 rounded-lg">
                    <p class="text-xs bg-gray-300 px-2">{{ $t('PassengerPage.non-Local') }}</p>
                  </div>
                </div>
                <div class="text-gray-400">
                  <div class="flex">
                    <p>{{ $t('PassengerPage.IdNumber') }} : </p>
                    <p class="pl-2">-</p>
                  </div>
                  <div class="flex">
                    <p>{{ $t('PassengerPage.phoneNumber') }} : </p>
                    <p class="pl-2">-</p>
                  </div>
                  <div class="flex">
                    <p>{{ $t('PassengerPage.birthday') }} : </p>
                    <p class="pl-2">-</p>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="place-content-center">
            <img src="/assets/icons/GreaterThan.png" alt="More" class="w-6 h-6"/>
          </div>
        </div>
        <div class="fixed inset-x-0 bottom-12 flex justify-center">
            <NuxtLink class="bg-green-400 text-white w-1/3 py-2 rounded flex justify-center">{{ $t('PassengerPage.addPassenger') }}</NuxtLink>
        </div>
      </div>
    </div>
  </template>
  
<script lang="ts">
export default defineComponent({
setup() {
    const localPath = useLocalePath();
    return {
        localPath,
    };
},

});
</script>