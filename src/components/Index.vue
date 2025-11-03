<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'
import LoadingOverlay from './LoadingOverlay.vue';
import GameCard from './GameCard.vue';
import type { Game } from '../types/game';


defineProps<{ msg: string }>()

const loading = ref(true)
const games = ref<Game[]>([])

const fetchGames = async () => {
  loading.value = true
  try {
    const response = await axios.get('/api/games')
    games.value = response.data
  } catch (error) {
    console.error('Error fetching games:', error)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchGames()
})
</script>

<template>
  <LoadingOverlay :show="loading" />
  <h1>{{ msg }}</h1>

  <div v-if="loading" class="loading">Loading games...</div>

  <div v-else class="games-container">
    <GameCard v-for="game in games" :key="game.id" :game="game" />
  </div>
</template>

<style scoped>
.games-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  padding: 1rem 0;
}
</style>


