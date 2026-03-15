<script setup lang="ts">
import BaseButton from './BaseButton.vue';

defineProps({
  isOpen: {
    type: Boolean,
    required: true,
  },
  title: {
    type: String,
    default: 'Melding',
  },
});

const emit = defineEmits(['close']);

function handleClose() {
  emit('close');
}
</script>

<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="handleClose">
    <div class="modal-content" role="dialog" aria-modal="true">
      <header class="modal-header">
        <h3 class="modal-title">{{ title }}</h3>
        <button class="close-button" @click="handleClose" aria-label="Lukk">×</button>
      </header>
      
      <div class="modal-body">
        <slot></slot>
      </div>

      <footer class="modal-footer">
        <slot name="footer">
          <BaseButton label="Lukk" variant="secondary" @click="handleClose" />
        </slot>
      </footer>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 1rem;
}

.modal-content {
  background: white;
  border-radius: 8px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
  border-bottom: 1px solid #e5e7eb;
}

.modal-title {
  margin: 0;
  font-size: 18px;
  color: #111827;
}

.close-button {
  background: transparent;
  border: none;
  font-size: 24px;
  line-height: 1;
  color: #6b7280;
  cursor: pointer;
  padding: 0;
}

.close-button:hover {
  color: #111827;
}

.modal-body {
  padding: 1.5rem;
  color: #4b5563;
  line-height: 1.5;
}

.modal-footer {
  padding: 1rem 1.5rem;
  border-top: 1px solid #e5e7eb;
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
}
</style>
