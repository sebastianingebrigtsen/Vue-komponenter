<script setup lang="ts">
import { ref } from 'vue';
import BaseButton from './components/BaseButton.vue';
import BaseInput from './components/BaseInput.vue';

function clickTest() {
  alert('Knappen er trykket på!');
}

const inputValue = ref('');
const lazyValue = ref('');

function handleSearch() {
  alert(`Søker etter: ${inputValue.value}`);
}
</script>

<template>
  <main>
    <h1>Vue Komponenter</h1>

    <section class="demo-section">
      <h2>Button test</h2>

      <div class="button-group">
        <BaseButton label="Lagre" @click="clickTest" />
        <BaseButton label="Avbryt" variant="secondary" />
        <BaseButton label="Slett" variant="danger" iconBefore="🗑" />
        <BaseButton label="BK" href="https://bk.no/tjenestekategorier/teknologi" iconAfter="↗" />
        <BaseButton label="Deaktivert" disabled />
        <BaseButton label="Egendefinert stil" customClass="big-button" />
      </div>
    </section>

    <section class="demo-section">
      <h2>Input test</h2>

      <div class="input-group">
        <BaseInput v-model="inputValue" placeholder="Vanlig input" />
        <BaseInput v-model="inputValue" placeholder="Nettisde" suffix=".no" />
        <BaseInput v-model="inputValue" placeholder="Søk" iconBefore="🔍" buttonLabel="Søk" @buttonClick="handleSearch" />
        <BaseInput v-model="lazyValue" placeholder="Lazy update" updateMode="lazy" />
      </div>

      <p>Vanlig verdi: {{ inputValue }}</p>
      <p>Lazy verdi: {{ lazyValue }}</p>
    </section>
  </main>
</template>

<style scoped>
main {
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
}

.intro {
  color: #4b5563;
  margin-bottom: 2rem;
}

.demo-section {
  background: white;
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  padding: 1.5rem;
  margin-bottom: 1.5rem;
}

.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-top: 1rem;
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin-top: 1rem;
}

.big-button {
  background-color: chocolate;
  font-size: 18px;
  padding: 12px 22px;
  border-radius: 10px;
}
</style>
