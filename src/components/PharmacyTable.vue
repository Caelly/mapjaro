<template>
  <div>
    <div class="pt-10 border-b border-gray-200">
      <div class="flex items-center justify-between">
        <h3 class="text-lg font-medium text-gray-900 flex items-center">
          📊 Résultats ({{ pharmacies.length }})
        </h3>
        <div v-if="loading" class="flex items-center text-sm text-gray-500">
          <div class="animate-spin rounded-full h-4 w-4 border-b-2 border-blue-500 mr-2"></div>
          Chargement...
        </div>
      </div>
    </div>

    <!-- TABLE -->
    <div class="overflow-x-auto">
      <table class="min-w-full divide-y divide-gray-200">
        <thead class="bg-gray-50">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              PHARMACIE
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              PRIX
            </th>
          </tr>
        </thead>
        <tbody class="bg-white divide-y divide-gray-200">
          <tr
            v-for="(pharmacy, index) in pharmacies"
            :key="index"
            class="hover:bg-gray-50 transition-colors cursor-pointer"
            @click="selectPharmacy(pharmacy)"
          >
            <td class="px-6 py-4 whitespace-nowrap">
              <div>
                <div class="text-sm font-medium text-gray-900">
                  {{ pharmacy.name }}
                </div>
                <div class="text-sm text-gray-500">
                  {{ pharmacy.address }}
                </div>
                <div class="text-sm text-gray-500">
                  {{ pharmacy.postal_code }} {{ pharmacy.city }}
                </div>
              </div>
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              <div class="text-sm font-medium text-green-600">{{ pharmacy.price }}€</div>
              <div class="text-xs text-gray-400">MAJ: {{ pharmacy.last_update }}</div>
            </td>
          </tr>
        </tbody>
      </table>

      <!-- État vide -->
      <div v-if="!loading && pharmacies.length === 0" class="text-center py-12">
        <div class="text-gray-400 text-lg mb-2">🔍</div>
        <h3 class="text-lg font-medium text-gray-900 mb-2">Aucun résultat</h3>
        <p class="text-gray-500">Essayez de modifier vos critères de recherche</p>
      </div>

      <!-- Loading state -->
      <div v-if="loading" class="text-center py-12">
        <div
          class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-500 mx-auto mb-4"
        ></div>
        <p class="text-gray-500">Recherche en cours...</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PharmacyTable',
  props: {
    pharmacies: {
      type: Array,
      default: () => [],
    },
    loading: {
      type: Boolean,
      default: false,
    },
  },
  emits: ['pharmacy-selected'],
  setup(props, { emit }) {
    const selectPharmacy = (pharmacy) => {
      emit('pharmacy-selected', pharmacy)
    }

    return {
      selectPharmacy,
    }
  },
}
</script>
