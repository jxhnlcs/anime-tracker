<template>
  <div class="results">
    <div class="result" v-for="anime in results" :key="anime.mal_id">
      <img :src="anime.images.jpg.image_url" alt="Anime image" />
      <div class="details">
        <h3>{{ anime.title }}</h3>
        <p :title="anime.synopsis" v-if="anime.synopsis">{{ anime.synopsis.slice(0, 180) }}...</p>
        <span class="flex-1"></span>
        <button class="button" @click="$emit('add', anime)">Adicionar na lista</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    results: Array,
  },
};
</script>

<style scoped>
.results {
  background-color: #fff;
  border-radius: 0.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-height: 480px;
  overflow-y: scroll;
  margin-bottom: 1.5rem;
}

.result {
  display: flex;
  margin: 1rem;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  transition: 0.4s;
}

.result img {
  width: 100px;
  border-radius: 1rem;
  margin-right: 1rem;
}

.details {
  flex: 1 1 0%;
  display: flex;
  align-items: flex-start;
  flex-direction: column;
}

.details h3 {
  font-size: 1.25rem;
  margin-bottom: 0.5rem;
}

.details p {
  font-size: 0.875rem;
  margin-bottom: 1rem;
}

.details .button {
  margin-left: auto;
  appearance: none;
  outline: none;
  border: none;
  background: none;
  cursor: pointer;

  display: block;
  padding: 0.5rem 1rem;
  background-image: linear-gradient(
    to right,
    rgb(58, 182, 240) 50%,
    rgb(32, 15, 128) 50%
  );
  background-size: 200%;
  color: white;
  font-size: 1.125rem;
  font-weight: bold;
  text-transform: uppercase;
  transition: 0.4s;
}

.details .button:hover {
  background-position: right;
}

.flex-1 {
  display: block;
  flex: 1 1 0%;
}
</style>