<script setup>
import { ref, computed, watch } from 'vue'

const selectedDose = ref('2,5 mg')
const doses = ['2,5 mg', '5 mg', '7,5 mg']

const formattedDose = computed(() => {
  return selectedDose.value.replace(',', '-').replace(' mg', '')
})

watch(formattedDose, (newVal) => {
  // Émet l’événement au parent
  emit('update:formattedDose', newVal)
})

const emit = defineEmits(['update:formattedDose'])

function selectDose(dose) {
  selectedDose.value = dose
}
</script>

<template>
  <div class="flex gap-2">
    <button
      v-for="dose in doses"
      :key="dose"
      @click="selectDose(dose)"
      :class="[
        'px-4 py-2 rounded-md font-medium transition-colors duration-200',
        selectedDose === dose
          ? 'bg-teal-500 text-white'
          : 'bg-white text-gray-700 border border-gray-300 hover:bg-gray-100',
      ]"
    >
      {{ dose }}
    </button>
  </div>
</template>
