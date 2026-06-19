<script setup>
import { computed, ref, watch } from 'vue'
import BookCard from './components/BookCard.vue'
import BookFilters from './components/BookFilters.vue'
import BookForm from './components/BookForm.vue'
import BookStats from './components/BookStats.vue'

const STORAGE_KEY = 'practice_14_vue_books'

const defaultBooks = [
  {
    id: 1,
    title: 'Мастер и Маргарита',
    author: 'Михаил Булгаков',
    genre: 'Роман',
    isRead: true,
    rating: 5,
  },
  {
    id: 2,
    title: 'Евгений Онегин',
    author: 'Александр Пушкин',
    genre: 'Классика',
    isRead: false,
    rating: 0,
  },
  {
    id: 3,
    title: 'Три товарища',
    author: 'Эрих Мария Ремарк',
    genre: 'Драма',
    isRead: false,
    rating: 0,
  },
]

const loadBooks = () => {
  try {
    const savedBooks = localStorage.getItem(STORAGE_KEY)

    if (!savedBooks) {
      return defaultBooks
    }

    const parsedBooks = JSON.parse(savedBooks)
    return Array.isArray(parsedBooks) ? parsedBooks : defaultBooks
  } catch {
    return defaultBooks
  }
}

const books = ref(loadBooks())
const activeFilter = ref('all')
const searchQuery = ref('')

const normalizedSearch = computed(() => searchQuery.value.trim().toLowerCase())

const filteredBooks = computed(() => {
  return books.value.filter((book) => {
    const matchesFilter =
      activeFilter.value === 'all' ||
      (activeFilter.value === 'unread' && !book.isRead) ||
      (activeFilter.value === 'read' && book.isRead)

    const matchesSearch =
      !normalizedSearch.value ||
      book.title.toLowerCase().includes(normalizedSearch.value) ||
      book.author.toLowerCase().includes(normalizedSearch.value)

    return matchesFilter && matchesSearch
  })
})

const stats = computed(() => {
  const readCount = books.value.filter((book) => book.isRead).length

  return {
    total: books.value.length,
    read: readCount,
    unread: books.value.length - readCount,
  }
})

const addBook = (book) => {
  books.value.unshift({
    id: Date.now(),
    ...book,
    isRead: false,
    rating: 0,
  })
}

const toggleRead = (bookId) => {
  const book = books.value.find((item) => item.id === bookId)

  if (book) {
    book.isRead = !book.isRead
    book.rating = book.isRead ? book.rating : 0
  }
}

const updateRating = ({ bookId, rating }) => {
  const book = books.value.find((item) => item.id === bookId)

  if (book && book.isRead) {
    book.rating = rating
  }
}

const deleteBook = (bookId) => {
  books.value = books.value.filter((book) => book.id !== bookId)
}

watch(
  books,
  (newBooks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newBooks))
  },
  { deep: true },
)
</script>

<template>
  <main class="app">
    <section class="hero">
      <p class="hero__label">Практическое занятие 14</p>
      <h1>Менеджер книг для чтения</h1>
      <p class="hero__description">
        Добавляйте книги, отмечайте прочитанные, ставьте оценки и сохраняйте
        личный список чтения прямо в браузере.
      </p>
    </section>

    <section class="workspace" aria-label="Рабочая область приложения">
      <div class="sidebar">
        <BookForm @add-book="addBook" />
        <BookStats :stats="stats" />
      </div>

      <div class="library">
        <BookFilters v-model:filter="activeFilter" v-model:search="searchQuery" />

        <div v-if="filteredBooks.length" class="book-grid">
          <BookCard
            v-for="book in filteredBooks"
            :key="book.id"
            :book="book"
            @toggle-read="toggleRead"
            @update-rating="updateRating"
            @delete-book="deleteBook"
          />
        </div>

        <div v-else class="empty-state">
          <h2>Книги не найдены</h2>
          <p>Попробуйте изменить фильтр, запрос поиска или добавить новую книгу.</p>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
.app {
  width: min(1180px, calc(100% - 32px));
  margin: 0 auto;
  padding: 40px 0;
}

.hero {
  margin-bottom: 32px;
  padding: 28px;
  color: #ffffff;
  background:
    linear-gradient(rgba(29, 69, 55, 0.82), rgba(25, 55, 48, 0.9)),
    url('https://images.unsplash.com/photo-1519682337058-a94d519337bc?auto=format&fit=crop&w=1600&q=80')
      center / cover;
  border-radius: 8px;
  box-shadow: 0 18px 45px rgba(31, 63, 56, 0.18);
}

.hero__label {
  margin: 0 0 10px;
  font-size: 0.88rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  color: #f2d076;
}

.hero h1 {
  max-width: 760px;
  margin: 0;
  font-size: clamp(2.2rem, 6vw, 4.8rem);
  line-height: 0.98;
  letter-spacing: 0;
}

.hero__description {
  max-width: 690px;
  margin: 18px 0 0;
  font-size: 1.08rem;
  line-height: 1.65;
  color: #f5f8f5;
}

.workspace {
  display: grid;
  grid-template-columns: minmax(280px, 360px) 1fr;
  gap: 24px;
  align-items: start;
}

.sidebar {
  display: grid;
  gap: 18px;
}

.library {
  display: grid;
  gap: 18px;
}

.book-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 18px;
}

.empty-state {
  padding: 36px;
  text-align: center;
  background: #ffffff;
  border: 1px solid #dce8dd;
  border-radius: 8px;
  box-shadow: 0 12px 28px rgba(40, 73, 59, 0.08);
}

.empty-state h2 {
  margin: 0 0 8px;
  font-size: 1.45rem;
  color: #214134;
}

.empty-state p {
  max-width: 460px;
  margin: 0 auto;
  line-height: 1.55;
  color: #60746a;
}

@media (max-width: 860px) {
  .app {
    width: min(100% - 24px, 720px);
    padding: 24px 0;
  }

  .workspace {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 520px) {
  .hero {
    padding: 22px;
  }

  .book-grid {
    grid-template-columns: 1fr;
  }
}
</style>
