<template>
    <div class="date-time-picker">
      <label for="datePicker">Select Date:</label>
      <input
        id="datePicker"
        type="date"
        :value="formattedDate"
        @input="updateDate(($event.target as HTMLInputElement).value || '')"
      />
  
      <label for="timePicker">Select Time:</label>
      <input
        id="timePicker"
        type="time"
        :value="time"
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
  