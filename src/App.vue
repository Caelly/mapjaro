<template>
  <div class="p-4">
    <!-- HEADER -->
    <Header
      :last-update="lastUpdate"
      :update-message="updateMessage"
      @contribute-click="showContributeModal = true"
    />
    <ContributeButton v-if="showContributeModal" @close="showContributeModal = false" />
    <!-- END HEADER -->

    <main class="max-w-7xl">
      <!-- LEFT SIDE -->
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
        <div class="space-y-6">
          <!-- Filters -->
          <div class="bg-white rounded-lg">
            <SearchFilters
              v-model:department="selectedDepartment"
              v-model:dosage="selectedDosage"
              @filter-change="handleFilterChange"
            />
          </div>
        </div>
      </div>
      <!-- END LEFT SIDE -->
      <div class="bg-white rounded-lg shadow">
        <PharmacyTable :pharmacies="filteredPharmacies" :loading="loading" />
      </div>
    </main>
    <Footer />
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import SearchFilters from './components/search/SearchFilters.vue'
import PharmacyTable from './components/PharmacyTable.vue'
import Header from './components/layout/Header.vue'
import ContributeButton from './components/layout/ContributeButton.vue'
import Footer from './components/layout/Footer.vue'
//import MapComponent from './components/MapComponent.vue'

// Import des données JSON
import mounjaro25Data from './data/mounjaro-2-5.json'
import mounjaro5Data from './data/mounjaro-5.json'
import mounjaro75Data from './data/mounjaro-7-5.json'

export default {
  name: 'App',
  components: {
    SearchFilters,
    PharmacyTable,
    //MapComponent,
    ContributeButton,
    Header,
    Footer,
  },
  setup() {
    const selectedDepartment = ref('')
    const selectedDosage = ref('2.5')
    const selectedPharmacy = ref(null)
    const showContributeModal = ref(false)
    const loading = ref(false)

    // Données combinées
    const allPharmaciesData = {
      2.5: mounjaro25Data,
      5: mounjaro5Data,
      7.5: mounjaro75Data,
    }

    // Pharmacies filtrées selon la dose et le département
    const filteredPharmacies = computed(() => {
      let pharmacies = allPharmaciesData[selectedDosage.value] || []

      if (selectedDepartment.value) {
        pharmacies = pharmacies.filter((pharmacy) =>
          pharmacy.postal_code.startsWith(selectedDepartment.value),
        )
      }

      return pharmacies.sort((a, b) => a.price - b.price)
    })

    const handleFilterChange = () => {
      loading.value = true
      selectedPharmacy.value = null

      // Simulation d'un délai de chargement
      setTimeout(() => {
        loading.value = false
      }, 300)
    }
    const handlePharmacySelected = (pharmacy) => {
      selectedPharmacy.value = pharmacy
    }

    return {
      selectedDepartment,
      selectedDosage,
      selectedPharmacy,
      showContributeModal,
      loading,
      filteredPharmacies,
      handleFilterChange,
      handlePharmacySelected,
    }
  },
}
</script>
