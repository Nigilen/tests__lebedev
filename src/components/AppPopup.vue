<script setup lang="ts">

const props = defineProps<{
  title: string;
  isPopupOpen: boolean;
}>();

const emit = defineEmits<{
  (e: 'close'): void
}>();

</script>

<template>
  <Transition name="popup">
    <div v-if="isPopupOpen" class="popup-overlay" @click="emit('close')">
      <div class="popup">
        <h2 class="popup__title">
          {{ title }}
        </h2>
        <button class="popup__close" @click="emit('close')" type="button">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M18 6L6 18" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M6 6L18 18" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>
    </div>
  </Transition>
</template>

<style scoped>

.popup-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  background-color: #fff;
  inline-size: clamp(20rem, 20vmin, 30rem);
  block-size: auto;
  aspect-ratio: 16 / 9;
  border-radius: 16px;
  container-type: inline-size;
}

.popup__title {
  font-size: 10cqmin;
  font-weight: bold;
}

.popup__close {
  position: absolute;
  inset-block-start: 2cqmin;
  inset-inline-end: 2cqmin;
  padding: 0;
  background-color: transparent;
}

.popup-enter-active,
.popup-leave-active {
  transition: opacity 0.3s ease;
}

.popup-enter-from,

.popup-leave-to {
  opacity: 0;
}

.popup-enter-to {
  opacity: 1;
}

.popup-leave-active {
  transition: opacity 0.3s ease;
}

.popup-leave-to {
  opacity: 0;
}

</style>