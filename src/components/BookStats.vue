<script setup>
defineProps({
  stats: {
    type: Object,
    required: true,
  },
})
</script>

<template>
  <section class="stats" aria-label="Статистика книг">
    <h2>Статистика</h2>

    <dl>
      <div>
        <dt>Всего книг</dt>
        <dd>{{ stats.total }}</dd>
      </div>
      <div>
        <dt>Прочитано</dt>
        <dd>{{ stats.read }}</dd>
      </div>
      <div>
        <dt>Непрочитано</dt>
        <dd>{{ stats.unread }}</dd>
      </div>
    </dl>
  </section>
</template>

<style scoped>
.stats {
  padding: 22px;
  background: #ffffff;
  border: 1px solid #dce8dd;
  border-radius: 8px;
  box-shadow: 0 12px 28px rgba(40, 73, 59, 0.08);
}

h2 {
  margin: 0 0 16px;
  font-size: 1.35rem;
  color: #214134;
}

dl {
  display: grid;
  gap: 10px;
  margin: 0;
}

dl div {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 12px 14px;
  background: #f6fbf7;
  border: 1px solid #dce8dd;
  border-radius: 8px;
}

dt {
  color: #60746a;
}

dd {
  min-width: 38px;
  margin: 0;
  font-size: 1.4rem;
  font-weight: 900;
  text-align: right;
  color: #2f6b55;
}
</style>
