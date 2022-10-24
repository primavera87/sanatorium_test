<template>
  <div class="card">
    <input type="date" v-model="departureDate">
    <button type="button" @click.prevent="decrement">-</button>
    <input type="number" v-model="count">
    <button type="button" @click.prevent="increment">+</button>
    <input type="date" v-model="dateOfEntry">
  </div>
</template>

<script setup lang="ts">
  import { ref, watch } from 'vue';
  import moment from 'moment';

  let count = ref(0)
  let departureDate = ref(moment().format('YYYY-MM-DD'))
  let dateOfEntry = ref(moment().format('YYYY-MM-DD'))

  function increment() {
    count.value++
  }

  function decrement() {
    if (count.value !== 0) {
      count.value--
    }
  }

  function parseDate(str: any) {
    let mdy = str.split('-')
    return new Date(mdy[0], mdy[1], mdy[2])
  }

  watch(count, (newValue, oldValue) => {
    dateOfEntry.value = moment(departureDate.value).add(count.value, "days").format('YYYY-MM-DD')
  })

  watch(departureDate, (newValue, oldValue) => {
    dateOfEntry.value = moment(departureDate.value).add(count.value, "days").format('YYYY-MM-DD')
  })

  watch(dateOfEntry, (newValue, oldValue) => {
    let start = moment(parseDate(departureDate.value).toString());
    let end = moment(parseDate(dateOfEntry.value).toString());
    count.value = end.diff(start, "days");
  })
</script>
