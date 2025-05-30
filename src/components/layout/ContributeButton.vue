<template>
  <div
    class="fixed inset-0 bg-black/80 flex items-center justify-center z-50 p-4"
    @click="closeModal"
  >
    <div
      class="bg-white rounded-lg shadow-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto"
      @click.stop
    >
      <div class="flex justify-between items-center p-6 border-b border-gray-200">
        <h2 class="text-xl font-semibold text-gray-900">Contribuer à MAPJARO</h2>
        <button @click="closeModal" class="text-gray-400 hover:text-gray-600 transition-colors">
          <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            ></path>
          </svg>
        </button>
      </div>

      <div class="p-6">
        <div class="max-w-none">
          <div class="py-6 border-b border-gray-100">
            <p class="text-lg font-medium text-gray-900 mb-3">
              💰 En envoyant les tarifs des pharmacies proches de chez vous
            </p>
            <p class="text-gray-700 mb-4 italic">
              Mapjaro est entièrement basé sur les données fournies par la communauté. Plus vous
              serez nombreux à contribuer, plus l'outil sera efficace !
            </p>
          </div>
          <div class="py-6 border-b border-gray-100">
            <p class="text-lg font-medium text-gray-900 mb-3">✍ En signalant les tarifs erronés</p>
            <p class="text-gray-700 mb-4 italic">
              Le médicament se popularise de plus en plus, les tarifs risquent de varier dans les
              mois à venir. Si vous constatez des écarts de prix entre la réalité et ce qui est
              écrit sur l'application, n'hésitez pas à le signaler.
            </p>
          </div>
          <div class="py-6">
            <p class="text-lg font-medium text-gray-900 mb-3 text-blue-600">
              💡 En faisant part des évolutions que vous aimeriez voir !
            </p>
            <p class="text-gray-700 mb-4 italic">
              Mapjaro est au service des personnes nécessitant des injections de Mounjaro. Si vous
              découvrez de nouveaux besoins, n'hésitez pas à les proposer.
            </p>
          </div>
        </div>
      </div>

      <div class="px-6 py-4 bg-gray-50 border-t border-gray-200 flex justify-between items-center">
        <div class="flex space-x-4 text-sm">
          <a href="mailto:contact@oraxya.com" class="text-blue-600 hover:text-blue-800">
            📧 contact@oraxya.com
          </a>
          <a href="https://www.instagram.com/oraxya_" class="text-blue-600 hover:text-blue-800">
            📱 @oraxya_ sur Instagram & Facebook
          </a>
        </div>
        <button
          @click="closeModal"
          class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg font-medium transition-colors"
        >
          Fermer
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContributeModal',
  emits: ['close'],
  setup(props, { emit }) {
    const closeModal = () => {
      emit('close')
    }

    return {
      closeModal,
    }
  },
}
</script>
