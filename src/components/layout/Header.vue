<template>
  <!-- TODO : adjust the right side -->
  <header>
    <div class="max-w-7xl mx-auto">
      <div class="flex flex-col sm:flex-row items-start sm:items-center gap-4 sm:gap-0">
        <!-- LEFT SIDE -->
        <div class="flex-1">
          <p class="text-2xl sm:text-3xl font-bold text-gray-900">MAPJARO</p>
          <h1 class="text-sm sm:text-base text-gray-600 mt-1">
            Trouve du Mounjaro au prix le plus bas !
          </h1>
        </div>

        <!-- RIGHT SIDE -->
        <div
          class="flex flex-col sm:flex-row items-start sm:items-center gap-3 sm:gap-4 sm:ml-auto w-full sm:w-auto"
        >
          <div class="flex items-center text-sm text-gray-500">
            <span>Dernière mise à jour le {{ lastUpdate }} : {{ updateMessage }}</span>
          </div>

          <button
            @click="handleContributeClick"
            class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg font-medium transition-colors duration-200 shadow-sm hover:shadow-md focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-offset-2"
          >
            CONTRIBUER
          </button>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  props: {
    lastUpdate: {
      type: String,
      default: '29/05',
    },
    updateMessage: {
      type: String,
      default: 'Ajout de nouvelles pharmacies en Île de France',
    },
  },
  emits: ['contribute-click'],
  setup(props, { emit }) {
    const handleContributeClick = () => {
      emit('contribute-click')
    }

    return {
      handleContributeClick,
    }
  },
}
</script>

<style scoped>
.header-gradient {
  background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
}
</style>
