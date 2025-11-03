<script setup lang="ts">
    defineProps<{ show: boolean }>()
</script>

<template>
  <transition name="fade">
    <div v-if="show" class="overlay">
      <div class="spinner"></div>
    </div>
  </transition>
</template>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(255, 255, 255, 0.8);
  display: grid;
  place-items: center;
  z-index: 9999;
}

/* simple rotating circle */
.spinner {
  width: 60px;
  height: 60px;
  border: 6px solid #ccc;
  border-top-color: #42b883;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.fade-enter-active,
.fade-leave-active { transition: opacity 0.2s; }
.fade-enter-from,
.fade-leave-to { opacity: 0; }
</style>
