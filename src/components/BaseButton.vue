<script setup lang="ts">
// Mulige stilvarianter for knappen.
type Variant = 'default' | 'secondary' | 'danger';

defineProps({
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
});

// Sender klikk-event videre til parent-komponenten.
const emit = defineEmits(['click']);

function handleClick(event: MouseEvent) {
  emit('click', event);
}
</script>

<template>
  <!--
    Vanlig knapp brukes når komponenten representerer en handling.
  -->
  <button v-if="!href" :class="['button', 'button-' + variant]" @click="handleClick">
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
  <a v-else :href="href" :class="['button', 'button-' + variant]">
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
}

/* Standard  */
.button-default {
  background: #1f2937;
  color: white;
}

.button-default:hover {
  background: #111827;
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
</style>
