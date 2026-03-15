<script setup lang="ts">
import { computed, ref } from 'vue';
import BaseButton from './BaseButton.vue';
import BaseInput from './BaseInput.vue';

// Grunnstruktur for hvert element i DataView.
type DataViewItem = {
  id: number;
  name: string;
  description: string;
  image: string;
};

const props = withDefaults(
  defineProps<{
    items: DataViewItem[];

    // Styrer om søkefeltet skal vises.
    showSearch?: boolean;

    // Gir mulighet for ekstra styling på hvert element.
    itemClass?: string;
  }>(),
  {
    showSearch: true,
  }
);

// Sender handlinger videre til parent-komponenten.
const emit = defineEmits(['edit', 'delete']);

// Styrer om elementene vises som grid eller tabell.
const viewMode = ref<'grid' | 'table'>('grid');

// Verdier for søk, sortering og valgte elementer.
const searchQuery = ref('');
const sortOrder = ref<'asc' | 'desc'>('asc');
const selectedIds = ref<number[]>([]);

// Filtrerer og sorterer elementene basert på søk og valgt sortering.
const filteredItems = computed(() => {
  let result = [...props.items];

  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase();
    result = result.filter((item) => item.name.toLowerCase().includes(query) || item.description.toLowerCase().includes(query));
  }

  result.sort((a, b) => {
    if (sortOrder.value === 'asc') {
      return a.name.localeCompare(b.name);
    }
    return b.name.localeCompare(a.name);
  });

  return result;
});

// Sender rediger-handling videre til parent.
function handleEdit(item: DataViewItem) {
  emit('edit', item);
}

// Sender slette-handling videre til parent.
function handleDelete(item: DataViewItem) {
  emit('delete', item);
}

// Legger til eller fjerner et element fra valgt-listen.
function toggleSelect(id: number) {
  if (selectedIds.value.includes(id)) {
    selectedIds.value = selectedIds.value.filter((itemId) => itemId !== id);
  } else {
    selectedIds.value.push(id);
  }
}

// Velger alle elementene som vises akkurat nå.
function selectAll() {
  selectedIds.value = filteredItems.value.map((item) => item.id);
}

// Fjerner alle valg.
function selectNone() {
  selectedIds.value = [];
}
</script>

<template>
  <div class="data-view">
    <div class="data-view-top">
      <div>
        <h3 class="data-view-title">Elementoversikt</h3>
        <p class="data-view-subtitle">Viser {{ filteredItems.length }} av {{ items.length }} elementer • Valgt: {{ selectedIds.length }}</p>
      </div>

      <div class="data-view-toolbar">
        <BaseButton label="Grid" :variant="viewMode === 'grid' ? 'default' : 'secondary'" @click="viewMode = 'grid'" />
        <BaseButton label="Tabell" :variant="viewMode === 'table' ? 'default' : 'secondary'" @click="viewMode = 'table'" />
        <BaseButton label="Velg alle" variant="secondary" @click="selectAll" />
        <BaseButton label="Fjern valg" variant="secondary" @click="selectNone" />

        <select v-model="sortOrder" class="sort-select">
          <option value="asc">Navn A–Å</option>
          <option value="desc">Navn Å–A</option>
        </select>
      </div>
    </div>

    <!--
      Søkefeltet vises bare når showSearch er true.
    -->
    <div v-if="showSearch" class="search-section">
      <label class="search-label">Søk i elementer</label>
      <BaseInput v-model="searchQuery" placeholder="Søk etter navn eller beskrivelse" iconBefore="🔍" />
    </div>

    <!--
      Grid-visning brukes når viewMode er satt til grid.
    -->
    <div v-if="viewMode === 'grid'" class="data-view-grid">
      <article v-for="item in filteredItems" :key="item.id" :class="['data-card', itemClass]">
        <div class="card-top">
          <label class="checkbox-label">
            <input type="checkbox" :checked="selectedIds.includes(item.id)" @change="toggleSelect(item.id)" />
            Velg
          </label>
        </div>

        <img :src="item.image" :alt="item.name" class="data-card-image" />

        <h3>{{ item.name }}</h3>
        <p>{{ item.description }}</p>

        <div class="data-card-actions">
          <BaseButton label="Rediger" variant="secondary" @click="handleEdit(item)" />
          <BaseButton label="Slett" variant="danger" @click="handleDelete(item)" />
        </div>
      </article>
    </div>

    <!--
      Tabell-visning brukes når viewMode er satt til table.
    -->
    <table v-else class="data-table">
      <thead>
        <tr>
          <th>Velg</th>
          <th>Bilde</th>
          <th>Navn</th>
          <th>Beskrivelse</th>
          <th>Handlinger</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in filteredItems" :key="item.id" :class="itemClass">
          <td>
            <input type="checkbox" :checked="selectedIds.includes(item.id)" @change="toggleSelect(item.id)" />
          </td>
          <td>
            <img :src="item.image" :alt="item.name" class="data-table-image" />
          </td>
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td>
            <div class="data-card-actions">
              <BaseButton label="Rediger" variant="secondary" @click="handleEdit(item)" />
              <BaseButton label="Slett" variant="danger" @click="handleDelete(item)" />
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <!--
      Vises når ingen elementer matcher søket.
    -->
    <p v-if="filteredItems.length === 0" class="empty-state">Ingen elementer matcher søket ditt.</p>
  </div>
</template>

<style scoped>
.data-view {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.data-view-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
  flex-wrap: wrap;
}

.data-view-title {
  margin: 0;
  font-size: 18px;
}

.data-view-subtitle {
  margin: 0.35rem 0 0;
  color: #6b7280;
  font-size: 14px;
}

.data-view-toolbar {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.search-section {
  padding: 1rem;
  border: 1px solid #e5e7eb;
  border-radius: 10px;
  background: #f9fafb;
}

.search-label {
  display: block;
  margin-bottom: 0.5rem;
  font-size: 14px;
  font-weight: 600;
  color: #111827;
}

.sort-select {
  padding: 10px 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-family: inherit;
  font-size: 14px;
  background: white;
}

.data-view-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 1rem;
}

.data-card {
  border: 1px solid #e5e7eb;
  border-radius: 10px;
  padding: 1rem;
  background: white;
}

.card-top {
  display: flex;
  justify-content: flex-start;
  margin-bottom: 0.5rem;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 14px;
  color: #374151;
}

.data-card-image {
  width: 100%;
  height: 140px;
  object-fit: cover;
  border-radius: 8px;
  margin: 0.5rem 0 0.75rem;
}

.data-card h3 {
  margin: 0 0 0.5rem;
  font-size: 16px;
}

.data-card p {
  margin: 0;
  color: #4b5563;
  font-size: 14px;
}

.data-card-actions {
  display: flex;
  gap: 0.75rem;
  margin-top: 1rem;
  flex-wrap: wrap;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  background: white;
}

.data-table th,
.data-table td {
  border: 1px solid #e5e7eb;
  padding: 0.75rem;
  text-align: left;
  vertical-align: middle;
}

.data-table th {
  background: #f9fafb;
}

.data-table-image {
  width: 70px;
  height: 50px;
  object-fit: cover;
  border-radius: 6px;
}

.empty-state {
  margin: 0;
  padding: 1rem;
  border: 1px dashed #d1d5db;
  border-radius: 10px;
  text-align: center;
  color: #6b7280;
  background: #f9fafb;
}
</style>
