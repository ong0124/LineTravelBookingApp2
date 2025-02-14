<template>
      <header class="py-4 border-b bg-green-400 flex items-center">
      <div>
        <BackBtn />
      </div>
      <h1 class="absolute left-1/2 transform -translate-x-1/2 text-xl font-bold capitalize">
        {{ $t('RefundPage.title') }}
      </h1>
    </header>

    <div class="px-6 pt-4 pb-4">
      <div class="bg-white p-4 rounded-lg flex flex-col mb-4">
        <div>
        <p class="font-bold text-lg">{{ $t('confirmationPage.tripInfo') }}</p>
        </div>
        <div class="flex border-b-2 py-2">
        <p>{{ $t('bookingStatus.dateTime') }}</p>
        <div class="flex ml-32">
          <p>{{ $t('bookingStatus.pier') }}</p>
          <Icon name="material-symbols-arrow-right-alt" class="mx-2 w-6 h-6"></Icon>
          <p>{{ $t('bookingStatus.airport') }}</p>
        </div>
      </div>
      <p>{{ $t('Booking.flightNumber') }} :  MF1544</p>
      <div class="flex py-2">
      <p >{{ $t('RefundPage.adultTicket') }}</p>
      <p class="mx-auto">{{ $t('RefundPage.childTicket') }}</p>
      <p class="mx-auto">{{ $t('RefundPage.paid') }} : &yen; {{ $t('confirmationPage.amount') }}</p>
      </div>
      <div class="flex">
        <p>{{ $t('Booking.contact') }} ：</p>
        <p>XXX先生</p>
      </div>
      <div class="flex py-2">
        <label class="self-center pr-2">{{ $t('RefundPage.refundType') }} :</label>
        <select 
          v-model="refundType" 
          class="w-1/3 h-8 border rounded-lg px-3 py-2 focus:ring-green-500"
        >
          <option value="full">{{ $t('RefundPage.changeOfPlans') }}</option>
          <option value="partial">{{ $t('RefundPage.tripCancellationOrAdjustment') }}</option>
          <option value="other">{{ $t('RefundPage.bookingError') }}</option>
          <option value="full">{{ $t('RefundPage.flightOrFerryIssue') }}</option>
          <option value="partial">{{ $t('RefundPage.systemError') }}</option>
          <option value="other">{{ $t('RefundPage.other') }}</option>
        </select>
      </div>
    <label class="">{{ $t('RefundPage.refundReason') }} :</label>
    <textarea
      v-model="refundReason"
      placeholder=" "
      class="w-full border rounded-lg px-3 py-2 h-24"
    ></textarea>
    <div class="flex justify-end pt-6">
          <NuxtLink class="bg-green-400 text-white w-1/6 py-2 rounded flex justify-center">{{ $t('RefundPage.applyRefund') }}</NuxtLink>
    </div>
    </div>
    </div>
</template>

<script>
export default {
  setup() {
    return {
      refundType: '',
      refundReason: ''
    };
  }
};
</script>