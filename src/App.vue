<template>
  <div class="p-4">
    <SelectMeasure @update:formattedDose="handleDose" />
    <InfoTable v-if="selectedDose" :dose="selectedDose" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import SelectMeasure from './components/search/SelectMeasure.vue'
import InfoTable from './components/InfoTable.vue'

const selectedDose = ref('')

function handleDose(dose) {
  selectedDose.value = dose // ex: "2-5"
}
</script>
