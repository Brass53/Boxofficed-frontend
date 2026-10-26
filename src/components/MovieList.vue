<script setup lang="ts">
import { ref, onMounted } from 'vue'

interface Movie {
  id: number
  name: string
  genre: string | null
  rating: number
  watched: boolean
}

const movies = ref<Movie[]>([])

onMounted(async () => {
  const baseUrl = import.meta.env.VITE_APP_BACKEND_BASE_URL
  try {
    const response = await fetch(`${baseUrl}/`)
    movies.value = await response.json()
  } catch (error) {
    console.error('Fehler beim Laden der Watchlist:', error)
  }
})
</script>

<template>
  <section class="movie-list">
    <h2>Meine Watchlist</h2>
    <ul>
      <li v-for="movie in movies" :key="movie.name">
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
li {
  padding: 0.4rem 0;
}
</style>