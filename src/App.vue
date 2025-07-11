<script setup lang="ts">
import dayjs from 'dayjs'
import { ref } from 'vue'
import type { Dayjs } from 'dayjs'
import VueTailwindDatePicker from './VueTailwindDatePicker.vue'

const availableDates = ref(['20241115', '20241116', '20241117'])
function parseAvailableDates(available: string[]) {
  return available
}

function complexDisableDate(date: Date) {
  const formattedDate = date?.toISOString()?.slice(0, 10)?.replace(/-/g, '') // 20241115
  // returns a boolean after checking if date is included in an array of dates in YYYYMMDD format
  return !parseAvailableDates(availableDates.value)?.includes(formattedDate)
}
const dateValue = ref({
  startDate: dayjs().format('YYYY-MM-DD HH:mm:ss'),
  endDate: dayjs().format('YYYY-MM-DD HH:mm:ss'),
})

const currentLocale = ref('es')
const locales = ['en', 'es', 'de']

function onClickSomething(e: Dayjs) {
  console.log(e)
}

function onSelectSomething(e: Dayjs) {
  console.log(e)
}
function dDate(date: Date) {
  return date < new Date()
}
</script>

<template>
  <div class="p-10 bg-sky-50 min-h-screen">
    <label>
      Choose one locale
      <select v-model="currentLocale" name="language" class="mb-6">
        <option v-for="locale in locales" :key="locale" :value="locale">
          {{ locale }}
        </option>
        >
      </select>
    </label>
    <div class="grid grid-rows-2 gap-4">
      <VueTailwindDatePicker
        v-model="dateValue" :i18n="currentLocale" @select-month="onSelectSomething($event)"
        @select-year="onSelectSomething($event)" @select-right-month="onSelectSomething($event)"
        @select-right-year="onSelectSomething($event)" @click-prev="onClickSomething($event)"
        @click-next="onClickSomething($event)" @click-right-prev="onClickSomething($event)"
        @click-right-next="onClickSomething($event)"
      />

      <VueTailwindDatePicker v-model="dateValue.startDate" as-single :i18n="currentLocale" />
      <div>
        <span>Disable dates in the past </span>
        <VueTailwindDatePicker v-model="dateValue" as-single :i18n="currentLocale" :disable-date="dDate" no-input />
      </div>
      <div>
        <span>More complex disable dates </span>
        <VueTailwindDatePicker v-model="dateValue" as-single :i18n="currentLocale" :disable-date="complexDisableDate" no-input />
      </div>
    </div>
  </div>
</template>
