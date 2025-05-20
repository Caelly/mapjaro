<template>
  <ul class="max-w-md divide-y divide-gray-200 dark:divide-gray-700">
    <li v-for="item in items" :key="item.name" class="py-4 sm:py-6">
      <div class="flex items-center space-x-6 rtl:space-x-reverse">
        <div class="shrink-0"></div>
        <div class="flex-1 min-w-0">
          <p class="text-base font-medium text-gray-900 truncate dark:text-white">
            {{ item.name }}
          </p>
          <p class="text-base font-medium text-gray-900 truncate dark:text-white">
            {{ item.address }}
          </p>
          <p class="italic text-sm text-gray-500 truncate dark:text-gray-400 mt-2">
            Mis à jour le {{ item.last_update }}
          </p>
        </div>
        <div class="inline-flex items-center text-base font-semibold text-gray-900 dark:text-white">
          {{ item.price }}€
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          name: 'Pharmacie du Progrès',
          address: '41 Route de Vienne, 69007 Lyon',
          last_update: '20/05/2025',
          price: 350,
        },
        // Ajoute d'autres éléments ici si besoin
      ],
    }
  },
}
</script>
