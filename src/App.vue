<template>
  <div class="p-4">
    <Header />
    <SearchLocation @update:selectedDepartment="department = $event" />
    <SelectMeasure @update:formattedDose="dose = $event" />
    <InfoTable :dose="dose" :department="department" />
    <Footer />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Header from './components/layout/Header.vue'
import Footer from './components/layout/Footer.vue'
import SearchLocation from './components/search/SearchLocation.vue'
import SelectMeasure from './components/search/SelectMeasure.vue'
import InfoTable from './components/InfoTable.vue'

const department = ref('')
const dose = ref('')

function handleDose(dose) {
  selectedDose.value = dose
}
</script>
