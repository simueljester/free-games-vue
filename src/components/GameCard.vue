<script setup lang="ts">
import type { Game } from '../types/game';

defineProps<{ game: Game }>()
</script>

<template>
    <div class="game-card">
      <img :src="game.thumbnail" :alt="game.title" class="game-thumbnail" />
      <div class="game-info">
        <h3>{{ game.title }}</h3>
        <p class="game-description">{{ game.short_description }}</p>
        <div class="game-meta">
          <span class="game-genre">{{ game.genre }}</span>
          <span class="game-platform">{{ game.platform }}</span>
        </div>
        <a :href="game.game_url" target="_blank" class="game-link">Play Now</a>
      </div>
    </div>
</template>

<style scoped>



.game-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  background: white;
  transition: transform 0.2s, box-shadow 0.2s;
}

.game-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.game-thumbnail {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.game-info {
  padding: 1rem;
}

.game-info h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.2rem;
  color: #333;
}

.game-description {
  color: #666;
  font-size: 0.9rem;
  margin: 0.5rem 0;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.game-meta {
  display: flex;
  gap: 0.5rem;
  margin: 0.75rem 0;
  flex-wrap: wrap;
}

.game-genre,
.game-platform {
  background: #f0f0f0;
  padding: 0.25rem 0.75rem;
  border-radius: 4px;
  font-size: 0.85rem;
  color: #555;
}

.game-link {
  display: inline-block;
  margin-top: 0.5rem;
  padding: 0.5rem 1rem;
  background: #42b983;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  font-weight: 500;
  transition: background 0.2s;
}

.game-link:hover {
  background: #35a372;
}
</style>