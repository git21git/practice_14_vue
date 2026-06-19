<script setup>
const props = defineProps({
  book: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['toggle-read', 'update-rating', 'delete-book'])

const setRating = (rating) => {
  if (!props.book.isRead) {
    return
  }

  emit('update-rating', {
    bookId: props.book.id,
    rating,
  })
}
</script>

<template>
  <article class="book-card" :class="{ 'book-card--read': book.isRead }">
    <div class="book-card__top">
      <span class="book-card__genre">{{ book.genre }}</span>
      <span v-if="book.isRead" class="book-card__status">Прочитано</span>
      <span v-else class="book-card__status book-card__status--unread">В планах</span>
    </div>

    <h2>{{ book.title }}</h2>
    <p class="book-card__author">{{ book.author }}</p>

    <div class="rating" :class="{ 'rating--disabled': !book.isRead }">
      <span class="rating__label">Оценка</span>
      <div class="rating__stars" aria-label="Оценка книги">
        <button
          v-for="star in 5"
          :key="star"
          type="button"
          :class="{ 'rating__star--active': star <= book.rating }"
          :disabled="!book.isRead"
          :aria-label="`Поставить оценку ${star}`"
          @click="setRating(star)"
        >
          ★
        </button>
      </div>
    </div>

    <div class="book-card__actions">
      <button type="button" class="book-card__toggle" @click="emit('toggle-read', book.id)">
        {{ book.isRead ? 'Вернуть в планы' : 'Отметить прочитанной' }}
      </button>
      <button type="button" class="book-card__delete" @click="emit('delete-book', book.id)">
        Удалить
      </button>
    </div>
  </article>
</template>

<style scoped>
.book-card {
  display: grid;
  gap: 14px;
  min-height: 276px;
  padding: 20px;
  background: #ffffff;
  border: 1px solid #dce8dd;
  border-left: 5px solid #cf9c38;
  border-radius: 8px;
  box-shadow: 0 12px 28px rgba(40, 73, 59, 0.08);
  transition:
    border-color 0.2s ease,
    transform 0.2s ease,
    box-shadow 0.2s ease;
}

.book-card:hover {
  box-shadow: 0 16px 34px rgba(40, 73, 59, 0.13);
  transform: translateY(-2px);
}

.book-card--read {
  background: #f6fbf7;
  border-left-color: #2f8f66;
}

.book-card__top {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  align-items: center;
  justify-content: space-between;
}

.book-card__genre,
.book-card__status {
  display: inline-flex;
  align-items: center;
  min-height: 28px;
  padding: 4px 9px;
  font-size: 0.78rem;
  font-weight: 800;
  color: #245643;
  background: #e6f1e9;
  border-radius: 999px;
}

.book-card__status {
  color: #ffffff;
  background: #2f8f66;
}

.book-card__status--unread {
  color: #6c4a03;
  background: #ffe7a8;
}

h2 {
  margin: 2px 0 0;
  font-size: 1.42rem;
  line-height: 1.22;
  color: #1f3c31;
}

.book-card__author {
  margin: 0;
  color: #60746a;
  line-height: 1.45;
}

.rating {
  display: grid;
  gap: 7px;
  margin-top: auto;
}

.rating__label {
  font-size: 0.9rem;
  font-weight: 800;
  color: #344b40;
}

.rating__stars {
  display: flex;
  gap: 4px;
}

.rating__stars button {
  width: 32px;
  height: 32px;
  padding: 0;
  font-size: 1.45rem;
  line-height: 1;
  color: #c4cfc8;
  background: transparent;
  border: 0;
  border-radius: 6px;
  transition:
    color 0.2s ease,
    transform 0.2s ease,
    background 0.2s ease;
}

.rating__stars button:hover:not(:disabled),
.rating__star--active {
  color: #d39a1d;
}

.rating__stars button:hover:not(:disabled) {
  background: #fff2ce;
  transform: scale(1.08);
}

.rating__stars button:disabled {
  cursor: not-allowed;
}

.rating--disabled {
  opacity: 0.62;
}

.book-card__actions {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 10px;
  align-items: center;
}

.book-card__actions button {
  min-height: 40px;
  padding: 9px 12px;
  font-weight: 800;
  border-radius: 8px;
  transition:
    background 0.2s ease,
    border-color 0.2s ease,
    color 0.2s ease;
}

.book-card__toggle {
  color: #ffffff;
  background: #2f6b55;
  border: 1px solid #2f6b55;
}

.book-card__toggle:hover {
  background: #245643;
  border-color: #245643;
}

.book-card__delete {
  color: #9f2f2f;
  background: #fff7f5;
  border: 1px solid #f0c6bf;
}

.book-card__delete:hover {
  color: #ffffff;
  background: #b33939;
  border-color: #b33939;
}

@media (max-width: 420px) {
  .book-card__actions {
    grid-template-columns: 1fr;
  }
}
</style>
