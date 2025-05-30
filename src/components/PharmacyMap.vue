<template>
  <div class="w-full h-[500px] rounded-2xl overflow-hidden shadow-xl">
    <div ref="mapContainer" style="width: 100%; height: 100%"></div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'
import L from 'leaflet'

const props = defineProps({
  pharmacies: Array,
})

// Container pour la carte
const mapContainer = ref(null)
let mapInstance = null
let markersLayer = null

const defaultPosition = [45.75, 4.85]

async function getCoords(pharmacy) {
  const query = encodeURIComponent(`${pharmacy.address}, ${pharmacy.city}`)
  const url = `https://nominatim.openstreetmap.org/search?format=json&q=${query}`

  try {
    const response = await fetch(url)
    const data = await response.json()
    if (data && data.length > 0) {
      return [parseFloat(data[0].lat), parseFloat(data[0].lon)]
    }
  } catch (e) {}
  return defaultPosition
}

async function addMarkers(pharmacies) {
  if (markersLayer) markersLayer.clearLayers()
  else markersLayer = L.layerGroup().addTo(mapInstance)

  for (const pharmacy of pharmacies) {
    const coords = await getCoords(pharmacy)
    const marker = L.marker(coords).bindPopup(`
        <b>${pharmacy.name}</b><br>
        ${pharmacy.address}, ${pharmacy.city} <br>
        Prix: <b>${pharmacy.price}€</b><br>
        Dernière maj: ${pharmacy.last_update}
      `)
    marker.addTo(markersLayer)
  }
  if (pharmacies.length > 0) {
    const allCoords = await Promise.all(pharmacies.map(getCoords))
    mapInstance.fitBounds(allCoords, { padding: [50, 50] })
  }
}

onMounted(async () => {
  mapInstance = L.map(mapContainer.value).setView(defaultPosition, 12)
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap contributors',
  }).addTo(mapInstance)
  await addMarkers(props.pharmacies)
})

watch(
  () => props.pharmacies,
  async (newVal) => {
    await addMarkers(newVal)
  },
)
</script>

<style scoped>
.leaflet-container {
  height: 100% !important;
  width: 100% !important;
}
</style>
