<script setup lang="ts">

interface Movie {
  name: string
  genre: string
  rating: number
  watched: boolean
}

// Beispiel-Entitäten: ein Array von Film-Objekten.
const movies: Movie[] = [
  { name: 'Inception',    genre: 'Sci-Fi',    rating: 5, watched: true },
  { name: 'The Matrix',   genre: 'Action',    rating: 4, watched: false },
  { name: 'Up',           genre: 'Animation', rating: 5, watched: true },
  { name: 'Interstellar', genre: 'Sci-Fi',    rating: 5, watched: false },
]
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