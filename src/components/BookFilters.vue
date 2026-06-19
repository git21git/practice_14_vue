<script setup>
defineProps({
  filter: {
    type: String,
    required: true,
  },
  search: {
    type: String,
    required: true,
  },
})

defineEmits(['update:filter', 'update:search'])

const filters = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' },
]
</script>

<template>
  <section class="filters" aria-label="Фильтры списка книг">
    <div class="filters__buttons">
      <button
        v-for="item in filters"
        :key="item.value"
        type="button"
        :class="{ 'filters__button--active': filter === item.value }"
        @click="$emit('update:filter', item.value)"
      >
        {{ item.label }}
      </button>
    </div>

    <label class="filters__search">
      <span>Поиск</span>
      <input
        :value="search"
        type="search"
        placeholder="Название или автор"
        @input="$emit('update:search', $event.target.value)"
      />
    </label>
  </section>
</template>

<style scoped>
.filters {
  display: grid;
  grid-template-columns: minmax(280px, 1fr) minmax(220px, 320px);
  gap: 14px;
  align-items: end;
  padding: 16px;
  background: #ffffff;
  border: 1px solid #dce8dd;
  border-radius: 8px;
  box-shadow: 0 12px 28px rgba(40, 73, 59, 0.08);
}

.filters__buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.filters__buttons button {
  min-height: 40px;
  padding: 9px 13px;
  font-weight: 800;
  color: #315246;
  background: #edf5ef;
  border: 1px solid #d5e5d8;
  border-radius: 8px;
  transition:
    background 0.2s ease,
    border-color 0.2s ease,
    color 0.2s ease;
}

.filters__buttons button:hover,
.filters__button--active {
  color: #ffffff;
  background: #2f6b55;
  border-color: #2f6b55;
}

.filters__search {
  display: grid;
  gap: 6px;
}

.filters__search span {
  font-size: 0.88rem;
  font-weight: 800;
  color: #344b40;
}

.filters__search input {
  width: 100%;
  min-height: 40px;
  padding: 9px 12px;
  color: #1b2733;
  background: #f8fbf8;
  border: 1px solid #cdded2;
  border-radius: 8px;
  outline: none;
  transition:
    border-color 0.2s ease,
    box-shadow 0.2s ease,
    background 0.2s ease;
}

.filters__search input:focus {
  background: #ffffff;
  border-color: #35735d;
  box-shadow: 0 0 0 3px rgba(53, 115, 93, 0.14);
}

@media (max-width: 720px) {
  .filters {
    grid-template-columns: 1fr;
  }
}
</style>
