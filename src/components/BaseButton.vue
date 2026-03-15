<script setup lang="ts">
// Mulige stilvarianter for knappen.
type Variant = 'default' | 'secondary' | 'danger';

const props = defineProps({
  label: String,

  // Bestemmer visuell variant av knappen.
  // Default brukes for primær handling.
  variant: {
    type: String as () => Variant,
    default: 'default',
  },

  // Hvis href er satt rendres komponenten som <a> i stedet for <button>.
  href: String,

  // Valgfrie ikoner før eller etter teksten.
  iconBefore: String,
  iconAfter: String,

  // Deaktiverer knappen og hindrer interaksjon
  disabled: Boolean,

  // Gir mulighet for ekstra styling fra parent-komponenten.
  // Vue støtter normalt class direkte på komponenter, men denne
  // prop-en gjør CSS passthrough eksplisitt i API-et.
  customClass: String,
});

// Sender klikk-event videre til parent-komponenten.
const emit = defineEmits(['click']);

function handleClick(event: MouseEvent) {
  if (props.disabled) {
    event.preventDefault();
    return;
  }
  emit('click', event);
}
</script>

<template>
  <!--
    Vanlig knapp brukes når komponenten representerer en handling.
  -->
  <button
    v-if="!href"
    type="button"
    :disabled="disabled"
    :class="['button', 'button-' + variant, { 'button-disabled': disabled }, customClass]"
    @click="handleClick"
  >
    <span v-if="iconBefore">{{ iconBefore }}</span>

    <span v-if="label">
      {{ label }}
    </span>

    <span v-if="iconAfter">{{ iconAfter }}</span>
  </button>

  <!--
    Hvis href er oppgitt brukes en anchor slik at komponenten
    også kan fungere som navigasjonslenke.
  -->
  <a
    v-else
    :href="disabled ? undefined : href"
    :class="['button', 'button-' + variant, { 'button-disabled': disabled }, customClass]"
    @click="handleClick"
  >
    <span v-if="iconBefore">{{ iconBefore }}</span>

    <span v-if="label">
      {{ label }}
    </span>

    <span v-if="iconAfter">{{ iconAfter }}</span>
  </a>
</template>

<style scoped>
/* Grunnstil for knapper */
.button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px;
  border-radius: 6px;
  border: none;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  text-decoration: none;
  transition: background 0.15s ease;
}

/* Standard  */
.button-default {
  background: #2563eb;
  color: white;
}

.button-default:hover {
  background: #1d4ed8;
}

/* Sekundær */
.button-secondary {
  background: white;
  border: 1px solid #d1d5db;
  color: #111827;
}

.button-secondary:hover {
  background: #f3f4f6;
}

/* Danger */
.button-danger {
  background: #b91c1c;
  color: white;
}

.button-danger:hover {
  background: #991b1b;
}
/* Disabled */
.button-disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
</style>
