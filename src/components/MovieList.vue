<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Movie {
  id: number
  name: string
  genre: string | null
  rating: number
  watched: boolean
}

const baseUrl = import.meta.env.VITE_APP_BACKEND_BASE_URL
const movies = ref<Movie[]>([])

// Formularfelder
const nameField = ref('')
const genreField = ref('')
const ratingField = ref(3)
const watchedField = ref(false)

// GET: beim Laden der Seite alle Filme holen
onMounted(async () => {
  try {
    const response = await fetch(`${baseUrl}/watchlist`)
    movies.value = await response.json()
  } catch (error) {
    console.error('Fehler beim Laden der Watchlist:', error)
  }
})

// POST: neuen Film ans Backend schicken
async function addMovie() {
  const data = {
    name: nameField.value,
    genre: genreField.value,
    rating: ratingField.value,
    watched: watchedField.value,
  }
  try {
    const response = await fetch(`${baseUrl}/watchlist`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data),
    })
    if (!response.ok) throw new Error(`HTTP ${response.status}`)
    const savedMovie: Movie = await response.json()
    movies.value.push(savedMovie)

    // Formular leeren
    nameField.value = ''
    genreField.value = ''
    ratingField.value = 3
    watchedField.value = false
  } catch (error) {
    console.error('Fehler beim Speichern:', error)
  }
}
</script>

<template>
  <section class="movie-list">
    <h2>Meine Watchlist</h2>

    <form @submit.prevent="addMovie">
      <input v-model="nameField" placeholder="Filmtitel" required />
      <input v-model="genreField" placeholder="Genre" />
      <input v-model.number="ratingField" type="number" min="1" max="5" />
      <label><input v-model="watchedField" type="checkbox" /> gesehen</label>
      <button type="submit">Hinzufügen</button>
    </form>

    <ul>
      <li v-for="movie in movies" :key="movie.id">
        <strong>{{ movie.name }}</strong> — {{ movie.genre }}
        · {{ movie.rating }}/5
        · {{ movie.watched ? 'gesehen' : 'noch nicht' }}
      </li>
    </ul>
  </section>
</template>

<style scoped>
.movie-list {
  max-width: 500px;
  margin: 2rem auto;
}
form {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
li {
  padding: 0.4rem 0;
}
</style>
