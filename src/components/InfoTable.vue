<script setup>
import { ref, watch, computed } from 'vue'

const props = defineProps({
  dose: String, // ex : '2-5', '5', '7-5'
  department: String, // ex : '42'
})

const rawItems = ref([])
const isLoading = ref(false)
const error = ref(null)

// Chargement des données en fonction de la dose
watch(
  () => props.dose,
  async (newDose) => {
    if (!newDose) return

    isLoading.value = true
    error.value = null
    rawItems.value = []

    try {
      const data = await import(`@/data/mounjaro-${newDose}.json`)
      rawItems.value = data.default
    } catch (err) {
      console.error('Erreur de chargement JSON :', err)
      error.value = `Données introuvables pour la dose ${newDose}`
    } finally {
      isLoading.value = false
    }
  },
  { immediate: true },
)

// Filtrage par département
const filteredItems = computed(() => {
  if (!props.department) return rawItems.value
  return rawItems.value.filter((pharmacy) => pharmacy.postal_code?.startsWith(props.department))
})
</script>

<template>
  <div class="mt-6">
    <p class="text-lg font-medium text-gray-800 mb-3">Résultats ({{ filteredItems.length }})</p>

    <div v-if="isLoading" class="text-gray-500">Chargement…</div>
    <div v-else-if="error" class="text-red-500">{{ error }}</div>

    <div v-else class="bg-white rounded-lg border border-gray-200 overflow-hidden">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Pharmacie
            </th>
            <th
              class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Adresse
            </th>
            <th
              class="px-4 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Prix
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr
            v-for="(pharmacy, index) in filteredItems"
            :key="index"
            :class="index < 2 ? 'bg-emerald-50' : ''"
          >
            <td class="px-4 py-4">
              <div class="font-medium text-gray-900">{{ pharmacy.name }}</div>
              <div class="text-sm text-gray-500">MàJ : {{ pharmacy.last_update }}</div>
            </td>
            <td class="px-4 py-4">
              <div class="text-sm text-gray-900">{{ pharmacy.address }}</div>
              <div class="text-sm text-gray-500">
                {{ pharmacy.postal_code }} {{ pharmacy.city }}
              </div>
            </td>
            <td class="px-4 py-4">
              <div class="text-lg font-bold text-emerald-600">{{ pharmacy.price }}€</div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
