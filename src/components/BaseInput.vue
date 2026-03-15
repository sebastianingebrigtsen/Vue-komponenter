<script setup lang="ts">
import BaseButton from './BaseButton.vue';

// Definerer hvordan inputen oppdaterer verdien.
// "eager" sender oppdatering for hver tast,
// mens "lazy" sender først ved change-event.
type UpdateMode = 'eager' | 'lazy';

const props = defineProps({
  // Verdien som bindes via v-model.
  modelValue: {
    type: String,
    default: '',
  },

  placeholder: String,

  // Valgfri tekst før eller etter input-feltet.
  prefix: String,
  suffix: String,

  // Valgfrie ikoner før eller etter input.
  iconBefore: String,
  iconAfter: String,

  // Hvis satt vises en integrert knapp til høyre i komponenten.
  buttonLabel: String,

  // Styrer når verdien sendes tilbake til parent-komponenten.
  updateMode: {
    type: String as () => UpdateMode,
    default: 'eager',
  },

  // Gir mulighet for ekstra styling fra parent-komponenten.
  // Vue støtter normalt class direkte på komponenter, men denne
  // prop-en gjør CSS passthrough eksplisitt i API-et.
  customClass: String,
});

// Sender oppdateringer til parent for v-model,
// samt event for den integrerte knappen.
const emit = defineEmits(['update:modelValue', 'buttonClick']);

// Oppdaterer verdien som sendes til parent.
function updateValue(event: Event) {
  const target = event.target as HTMLInputElement;
  emit('update:modelValue', target.value);
}

// Håndterer input-event ved eager oppdatering.
function handleInput(event: Event) {
  if (props.updateMode === 'eager') {
    updateValue(event);
  }
}

// Håndterer change-event ved lazy oppdatering.
function handleChange(event: Event) {
  if (props.updateMode === 'lazy') {
    updateValue(event);
  }
}

// Sender event når den integrerte knappen trykkes.
function handleButtonClick() {
  emit('buttonClick');
}
</script>

<template>
  <div :class="['input-wrapper', customClass]">
    <span v-if="iconBefore" class="input-extra">{{ iconBefore }}</span>
    <span v-if="prefix" class="input-extra">{{ prefix }}</span>

    <input class="input" :value="props.modelValue" :placeholder="placeholder" @input="handleInput" @change="handleChange" />

    <span v-if="suffix" class="input-extra">{{ suffix }}</span>
    <span v-if="iconAfter" class="input-extra">{{ iconAfter }}</span>

    <BaseButton v-if="buttonLabel" :label="buttonLabel" customClass="input-button" @click="handleButtonClick" />
  </div>
</template>

<style scoped>
/* Wrapper rundt input og tilhørende elementer */

.input-wrapper {
  display: flex;
  align-items: center;
  width: 100%;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  background: white;
  overflow: hidden;
}

/* input-feltet */

.input {
  flex: 1;
  border: none;
  outline: none;
  padding: 10px 14px;
  font-size: 14px;
  font-family: inherit;
  background: transparent;
}

/* Prefix, suffix og ikoner */

.input-extra {
  padding: 0 12px;
  color: #6b7280;
  font-size: 14px;
}

/* integrert knapp */

.input-button {
  border-radius: 0;
}
</style>
