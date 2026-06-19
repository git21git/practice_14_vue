<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-book'])

const title = ref('')
const author = ref('')
const genre = ref('')

const clearForm = () => {
  title.value = ''
  author.value = ''
  genre.value = ''
}

const submitBook = () => {
  const preparedBook = {
    title: title.value.trim(),
    author: author.value.trim(),
    genre: genre.value.trim(),
  }

  if (!preparedBook.title || !preparedBook.author || !preparedBook.genre) {
    return
  }

  emit('add-book', preparedBook)
  clearForm()
}
</script>

<template>
  <form class="book-form" @submit.prevent="submitBook">
    <div class="book-form__header">
      <p>Новая книга</p>
      <h2>Добавить в список</h2>
    </div>

    <label>
      <span>Название</span>
      <input v-model="title" type="text" placeholder="Например, Маленький принц" />
    </label>

    <label>
      <span>Автор</span>
      <input v-model="author" type="text" placeholder="Например, Антуан де Сент-Экзюпери" />
    </label>

    <label>
      <span>Жанр</span>
      <input v-model="genre" type="text" placeholder="Например, повесть" />
    </label>

    <button type="submit">Добавить книгу</button>
  </form>
</template>

<style scoped>
.book-form {
  display: grid;
  gap: 16px;
  padding: 22px;
  background: #ffffff;
  border: 1px solid #dce8dd;
  border-radius: 8px;
  box-shadow: 0 12px 28px rgba(40, 73, 59, 0.08);
}

.book-form__header p {
  margin: 0 0 4px;
  font-size: 0.78rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  color: #35735d;
}

.book-form__header h2 {
  margin: 0;
  font-size: 1.45rem;
  color: #214134;
}

label {
  display: grid;
  gap: 7px;
}

label span {
  font-size: 0.92rem;
  font-weight: 700;
  color: #344b40;
}

input {
  width: 100%;
  min-height: 44px;
  padding: 10px 12px;
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

input:focus {
  background: #ffffff;
  border-color: #35735d;
  box-shadow: 0 0 0 3px rgba(53, 115, 93, 0.14);
}

button {
  min-height: 46px;
  padding: 11px 16px;
  font-weight: 800;
  color: #ffffff;
  background: #2f6b55;
  border: 0;
  border-radius: 8px;
  transition:
    transform 0.2s ease,
    background 0.2s ease,
    box-shadow 0.2s ease;
}

button:hover {
  background: #245643;
  box-shadow: 0 10px 22px rgba(47, 107, 85, 0.22);
  transform: translateY(-1px);
}
</style>
