<template>
  <div class="flex flex-col gap-2 w-36">
    <input
      id="datePicker"
      type="date"
      :value="formattedDate"
      :disabled="isDisabled"
      @input="updateDate(($event.target as HTMLInputElement).value || '')"
      class="border-2 rounded-sm"
    />


    <input
      id="timePicker"
      type="time"
      :value="time"
      :disabled="isDisabled"
      @input="updateTime(($event.target as HTMLInputElement).value || '')"
    />

    <p class="selected-datetime">Selected: {{ fullDateTime }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';

export default defineComponent({
  name: 'DateTimePicker',
  setup() {
    const date = ref<string>('');
    const time = ref<string>('');
    const isDisabled = ref<boolean>(false); // 控制禁用状态

    const formattedDate = computed(() => {
      const currentDate = date.value ? new Date(date.value) : new Date();
      return currentDate.toISOString().split('T')[0];
    });

    const fullDateTime = computed(() => {
      return date.value && time.value
        ? `${date.value} ${time.value}`
        : 'No date and time selected';
    });

    const updateDate = (value: string) => {
      date.value = value;
    };

    const updateTime = (value: string) => {
      time.value = value;
    };

    return {
      formattedDate,
      time,
      fullDateTime,
      updateDate,
      updateTime,
      isDisabled, 
    };
  },
});
</script>

<style scoped>
.date-time-picker {
  display: flex;
  flex-direction: column;
  gap: 10px;
  width: 300px;
  margin: 0 auto;
}

label {
  font-weight: bold;
}

input {
  padding: 5px;
  font-size: 16px;
}

.selected-datetime {
  margin-top: 10px;
  font-style: italic;
  color: #555;
}
</style>
