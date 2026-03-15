<script setup lang="ts">
import BaseButton from './components/BaseButton.vue';

function clickTest() {
  alert('Knappen er trykket på!');
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
}

.button-group {
  display: flex;
  flex-wrap: wrap;
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
