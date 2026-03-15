<script setup lang="ts">
import { ref } from 'vue';
import BaseButton from './components/BaseButton.vue';
import BaseInput from './components/BaseInput.vue';
import DataView from './components/DataView.vue';
import { dummyItems } from './data/dummyData';

function clickTest() {
  alert('Knappen er trykket på!');
}

// Uavhengige tilstander for hver av input-testene slik at de fungerer hver for seg
const basicValue = ref('');
const iconValue = ref('');
const prefixSuffixValue = ref('');
const buttonValue = ref('');
const lazyValue = ref('');
const customCssValue = ref('');

function handleSearch() {
  alert(`Søker etter: ${buttonValue.value}`);
}
const items = ref(dummyItems);

function handleEdit(item: { name: string }) {
  alert(`Redigerer: ${item.name}`);
}

function handleDelete(item: { name: string }) {
  alert(`Sletter: ${item.name}`);
}
</script>

<template>
  <main>
      <h1>Vue Komponenter</h1>
    <!-- 
      Test-seksjon for Knappe-komponenten.
      Viser frem ulike tilstander og funksjoner som ikoner, lenker og deaktivering.
    -->
    <section class="demo-section">
      <div class="section-header">
        <h2>Button</h2>
        <p>Støtter ulike varianter, ikoner, lenker og deaktivert tilstand.</p>
      </div>

      <div class="button-group">
        <BaseButton label="Lagre" @click="clickTest" />
        <BaseButton label="Avbryt" variant="secondary" />
        <BaseButton label="Slett" variant="danger" iconBefore="🗑" />
        <BaseButton label="BK" href="https://bk.no/tjenestekategorier/teknologi" iconAfter="↗" />
        <BaseButton label="Deaktivert" disabled />
        <BaseButton label="Egendefinert stil" customClass="orange-button" />
      </div>
    </section>

    <!-- 
      Test-seksjon for Input-komponenten.
      Hver boks representerer et spesifikt krav fra oppgaveteksten og har sin egen v-model.
    -->
    <section class="demo-section">
      <div class="section-header">
        <h2>Input</h2>
        <p>Fleksibelt tekstfelt med støtte for ikoner, pre-/suffix, integrert knapp og lazy update.</p>
      </div>

      <div class="input-demo-grid">
        <div class="input-demo-item">
          <!-- Her bindes verdien direkte og oppdateres på hvert tastetrykk (eager update ved utelatt updateMode-prop) -->
          <label>1. Standard input</label>
          <BaseInput v-model="basicValue" placeholder="Skriv noe her..." />
          <p class="demo-result">Verdi: <strong>{{ basicValue }}</strong></p>
        </div>

        <div class="input-demo-item">
          <label>2. Ikoner (Før og/eller etter)</label>
          <BaseInput v-model="iconValue" placeholder="Brukernavn" iconBefore="👤" iconAfter="✓" />
          <p class="demo-result">Verdi: <strong>{{ iconValue }}</strong></p>
        </div>

        <div class="input-demo-item">
          <label>3. Prefix og Suffix</label>
          <BaseInput v-model="prefixSuffixValue" placeholder="Beløp" prefix="kr" suffix=",00" />
          <p class="demo-result">Verdi: <strong>{{ prefixSuffixValue }}</strong></p>
        </div>

        <div class="input-demo-item">
          <label>4. Integrert knapp</label>
          <BaseInput v-model="buttonValue" placeholder="Søk her" iconBefore="🔍" buttonLabel="Søk" @buttonClick="handleSearch" />
          <p class="demo-result">Verdi: <strong>{{ buttonValue }}</strong></p>
        </div>

        <div class="input-demo-item">
          <!-- Viser hvordan updateMode="lazy" fører til at oppdatering kun skjer når feltet mister fokus eller man trykker enter -->
          <label>5. Lazy update</label>
          <BaseInput v-model="lazyValue" placeholder="Oppdateres ved enter/klikk utenfor..." updateMode="lazy" />
          <p class="demo-result">Verdi: <strong>{{ lazyValue }}</strong></p>
        </div>

        <div class="input-demo-item">
          <!-- customClass viser CSS passthrough der en tilpasset stil (definert nederst i fila) brukes på den underliggende inputen -->
          <label>6. CSS Passthrough</label>
          <BaseInput v-model="customCssValue" placeholder=" Input" customClass="orange-input" iconBefore="✨" />
          <p class="demo-result">Verdi: <strong>{{ customCssValue }}</strong></p>
        </div>
      </div>
    </section>

    <!-- 
      Test-seksjon for DataView-komponenten.
      Sender inn DummyData, en custom-class for radene, og fanger opp eventuelle redigerings- og slettekall
    -->
    <section class="demo-section">
      <div class="section-header">
        <h2>Data View</h2>
        <p>Visning av data i grid- eller tabellformat med søk, sortering og valg.</p>
      </div>

      <DataView :items="items" itemClass="custom-item" @edit="handleEdit" @delete="handleDelete" />
    </section>
  </main>
</template>

<style scoped>
main {
  max-width: 1000px;
  margin: 0 auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
  color: #333;
}

.page-header {
  margin-bottom: 2rem;
  border-bottom: 1px solid #ccc;
  padding-bottom: 1rem;
}

.page-header h1 {
  margin: 0 0 0.5rem 0;
}

.page-description {
  margin: 0;
  color: #666;
}

.demo-section {
  border: 1px solid #ccc;
  padding: 1.5rem;
  margin-bottom: 2rem;
  background: #fafafa;
}

.section-header {
  margin-bottom: 1rem;
}

.section-header h2 {
  margin: 0 0 5px 0;
}

.section-header p {
  margin: 0;
  color: #666;
  font-size: 14px;
}

.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.input-demo-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.input-demo-item {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  background: #fff;
  padding: 1rem;
  border: 1px solid #ddd;
}

.input-demo-item label {
  font-weight: bold;
  font-size: 14px;
  color: #333;
}

.demo-result {
  margin: 0;
  font-size: 14px;
  color: green;
}

.orange-input {
  border: 2px solid orange;
  background-color: #fffaf0;
  color: #b76c00;
}

.orange-button {
  background-color: orange;
  color: white;
  font-size: 16px;
  padding: 10px 20px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
}

.orange-button:hover {
  background-color: darkorange;
}

.custom-item {
  border: 4px solid orange;
  background-color: #fff8f0;
}
</style>
