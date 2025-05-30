<template>
  <div class="p-4">
    <Header
      :last-update="lastUpdate"
      :update-message="updateMessage"
      @contribute-click="showContributeModal = true"
    />
    <ContributeButton v-if="showContributeModal" @close="showContributeModal = false" />
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
import ContributeButton from './components/layout/ContributeButton.vue'

const department = ref('')
const dose = ref('')
const showContributeModal = ref(false)

function handleDose(dose) {
  selectedDose.value = dose
}
</script>
