<template>
  <div class="space-y-4">
    <!-- Département -->
    <div class="pt-10">
      <label class="flex items-center text-xl font-bold mb-2"> Département </label>

      <div class="relative">
        <input
          v-model="departmentInput"
          @input="handleDepartmentInput"
          @focus="showSuggestions = true"
          @blur="handleBlur"
          type="text"
          placeholder="Rechercher un département..."
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
        />
        <div
          v-if="showSuggestions && filteredDepartments.length > 0"
          class="absolute z-10 w-full mt-1 bg-white border border-gray-200 rounded-lg shadow-lg max-h-48 overflow-y-auto"
        >
          <div
            v-for="dept in filteredDepartments"
            :key="dept.code"
            @mousedown="selectDepartment(dept)"
            class="flex items-center px-4 py-2 hover:bg-gray-50 cursor-pointer"
          >
            <span class="font-medium text-gray-900">{{ dept.code }} - </span>
            <span class="ml-2 text-gray-600"> {{ dept.name }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Dosage -->
    <div class="pt-6">
      <label class="flex items-center text-xl font-bold mb-2">Dosage </label>
      <div class="flex space-x-2">
        <button
          v-for="dose in dosages"
          :key="dose"
          @click="selectDosage(dose)"
          :class="[
            'px-4 py-2 rounded-lg font-medium transition-colors',
            selectedDosage === dose
              ? 'bg-green-100 text-green-800 border-2 border-green-500'
              : 'bg-gray-100 text-gray-700 border border-gray-300 hover:bg-gray-200',
          ]"
        >
          {{ dose }} mg
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, watch } from 'vue'

export default {
  name: 'SearchFilters',
  props: {
    department: {
      type: String,
      default: '',
    },
    dosage: {
      type: String,
      default: '2.5',
    },
  },
  emits: ['update:department', 'update:dosage', 'filter-change'],
  setup(props, { emit }) {
    const departmentInput = ref('')
    const showSuggestions = ref(false)
    const selectedDosage = ref(props.dosage)

    // Liste des départements français
    const departments = [
      { code: '01', name: 'Ain' },
      { code: '02', name: 'Aisne' },
      { code: '03', name: 'Allier' },
      { code: '04', name: 'Alpes-de-Haute-Provence' },
      { code: '05', name: 'Hautes-Alpes' },
      { code: '06', name: 'Alpes-Maritimes' },
      { code: '07', name: 'Ardèche' },
      { code: '08', name: 'Ardennes' },
      { code: '09', name: 'Ariège' },
      { code: '10', name: 'Aube' },
      { code: '11', name: 'Aude' },
      { code: '12', name: 'Aveyron' },
      { code: '13', name: 'Bouches-du-Rhône' },
      { code: '14', name: 'Calvados' },
      { code: '15', name: 'Cantal' },
      { code: '16', name: 'Charente' },
      { code: '17', name: 'Charente-Maritime' },
      { code: '18', name: 'Cher' },
      { code: '19', name: 'Corrèze' },
      { code: '21', name: "Côte-d'Or" },
      { code: '22', name: "Côtes-d'Armor" },
      { code: '23', name: 'Creuse' },
      { code: '24', name: 'Dordogne' },
      { code: '25', name: 'Doubs' },
      { code: '26', name: 'Drôme' },
      { code: '27', name: 'Eure' },
      { code: '28', name: 'Eure-et-Loir' },
      { code: '29', name: 'Finistère' },
      { code: '30', name: 'Gard' },
      { code: '31', name: 'Haute-Garonne' },
      { code: '32', name: 'Gers' },
      { code: '33', name: 'Gironde' },
      { code: '34', name: 'Hérault' },
      { code: '35', name: 'Ille-et-Vilaine' },
      { code: '36', name: 'Indre' },
      { code: '37', name: 'Indre-et-Loire' },
      { code: '38', name: 'Isère' },
      { code: '39', name: 'Jura' },
      { code: '40', name: 'Landes' },
      { code: '41', name: 'Loir-et-Cher' },
      { code: '42', name: 'Loire' },
      { code: '43', name: 'Haute-Loire' },
      { code: '44', name: 'Loire-Atlantique' },
      { code: '45', name: 'Loiret' },
      { code: '46', name: 'Lot' },
      { code: '47', name: 'Lot-et-Garonne' },
      { code: '48', name: 'Lozère' },
      { code: '49', name: 'Maine-et-Loire' },
      { code: '50', name: 'Manche' },
      { code: '51', name: 'Marne' },
      { code: '52', name: 'Haute-Marne' },
      { code: '53', name: 'Mayenne' },
      { code: '54', name: 'Meurthe-et-Moselle' },
      { code: '55', name: 'Meuse' },
      { code: '56', name: 'Morbihan' },
      { code: '57', name: 'Moselle' },
      { code: '58', name: 'Nièvre' },
      { code: '59', name: 'Nord' },
      { code: '60', name: 'Oise' },
      { code: '61', name: 'Orne' },
      { code: '62', name: 'Pas-de-Calais' },
      { code: '63', name: 'Puy-de-Dôme' },
      { code: '64', name: 'Pyrénées-Atlantiques' },
      { code: '65', name: 'Hautes-Pyrénées' },
      { code: '66', name: 'Pyrénées-Orientales' },
      { code: '67', name: 'Bas-Rhin' },
      { code: '68', name: 'Haut-Rhin' },
      { code: '69', name: 'Rhône' },
      { code: '70', name: 'Haute-Saône' },
      { code: '71', name: 'Saône-et-Loire' },
      { code: '72', name: 'Sarthe' },
      { code: '73', name: 'Savoie' },
      { code: '74', name: 'Haute-Savoie' },
      { code: '75', name: 'Paris' },
      { code: '76', name: 'Seine-Maritime' },
      { code: '77', name: 'Seine-et-Marne' },
      { code: '78', name: 'Yvelines' },
      { code: '79', name: 'Deux-Sèvres' },
      { code: '80', name: 'Somme' },
      { code: '81', name: 'Tarn' },
      { code: '82', name: 'Tarn-et-Garonne' },
      { code: '83', name: 'Var' },
      { code: '84', name: 'Vaucluse' },
      { code: '85', name: 'Vendée' },
      { code: '86', name: 'Vienne' },
      { code: '87', name: 'Haute-Vienne' },
      { code: '88', name: 'Vosges' },
      { code: '89', name: 'Yonne' },
      { code: '90', name: 'Territoire de Belfort' },
      { code: '91', name: 'Essonne' },
      { code: '92', name: 'Hauts-de-Seine' },
      { code: '93', name: 'Seine-Saint-Denis' },
      { code: '94', name: 'Val-de-Marne' },
      { code: '95', name: "Val-d'Oise" },
    ]

    const dosages = ['2.5', '5', '7.5']

    const filteredDepartments = computed(() => {
      if (!departmentInput.value) return departments.slice(0, 10)

      const query = departmentInput.value.toLowerCase()
      return departments
        .filter((dept) => dept.code.includes(query) || dept.name.toLowerCase().includes(query))
        .slice(0, 10)
    })

    const handleDepartmentInput = () => {
      showSuggestions.value = true
    }

    const selectDepartment = (department) => {
      departmentInput.value = `${department.code} - ${department.name}`
      showSuggestions.value = false
      emit('update:department', department.code)
      emit('filter-change')
    }

    const handleBlur = () => {
      setTimeout(() => {
        showSuggestions.value = false
      }, 200)
    }

    const selectDosage = (dosage) => {
      selectedDosage.value = dosage
      emit('update:dosage', dosage)
      emit('filter-change')
    }

    watch(
      () => props.dosage,
      (newVal) => {
        selectedDosage.value = newVal
      },
    )

    return {
      departmentInput,
      showSuggestions,
      selectedDosage,
      departments,
      dosages,
      filteredDepartments,
      handleDepartmentInput,
      selectDepartment,
      handleBlur,
      selectDosage,
    }
  },
}
</script>
