<script setup>
import { ref } from 'vue'

const emit = defineEmits(['update:selectedDepartment'])

const search = ref('')

function updateDepartment() {
  const match = search.value.match(/^(\d{2})/)
  const department = match ? match[1] : ''
  emit('update:selectedDepartment', department)
}
</script>

<template>
  <div class="my-4">
    <input
      id="search"
      v-model="search"
      @input="updateDepartment"
      type="text"
      placeholder="Code Postal ou N° de département"
      class="border border-gray-300 rounded-md px-3 py-2 w-full"
    />
  </div>
</template>
