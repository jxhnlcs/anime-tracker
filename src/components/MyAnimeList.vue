<template>
  <div class="myanime">
    <h2>Meus animes</h2>
    <p v-if="animes.length === 0" class="no-anime-message">Nenhum anime adicionado na lista</p>
    <div v-for="anime in animes" :key="anime.id" class="anime">
      <img :src="anime.image" />

      <h3>{{ anime.title }}</h3>

      <button class="remove-button" @click="$emit('remove', anime)">x</button>
      <div class="flex-1"></div>

      <div class="episode-controls">

        <span class="status" :class="{ completed: anime.watched_episodes === anime.total_episodes, watching: anime.watched_episodes < anime.total_episodes, }"> {{ anime.watched_episodes === anime.total_episodes ? "Concluído" : "Assistindo" }} </span>

        <span class="episodes">{{ anime.watched_episodes }} / {{ anime.total_episodes }}</span>

        <button v-if="anime.watched_episodes > 0" @click="$emit('decrease', anime)" class="button">-</button>

        <button v-if="anime.total_episodes !== anime.watched_episodes" @click="$emit('increase', anime)" class="button">+</button>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  props: {
    animes: Array,
  },
};
</script>

<style scoped>
.myanime h2 {
  color: #888;
  font-weight: 400;
  margin-bottom: 1.5rem;
}

.myanime .anime {
  display: flex;
  align-items: center;
  margin-bottom: 1.5rem;
  background-color: #fff;
  padding: 1rem;
  border-radius: 0.5rem;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  position: relative;
}

.anime img {
  width: 72px;
  height: 72px;
  object-fit: cover;
  border-radius: 1rem;
  margin-right: 1rem;
}

.anime h3 {
  color: #888;
  font-size: 1.125rem;
}

.anime .flex-1 {
  flex: 1;
}

.episode-controls {
  display: flex;
  align-items: center;
}

.anime .status {
  margin-right: 1rem;
  font-weight: bold;
}

.anime .status.completed {
  color: green;
}

.anime .status.watching {
  color: rgb(230, 170, 74);
}

.anime .episodes {
  margin-right: 1rem;
  color: #888;
}

.anime .button {
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

.anime .button:first-of-type {
  margin-right: 0.5rem;
}

.anime .button:hover {
  background-position: right;
}

.anime .remove-button {
  position: absolute;
  top: 1px;
  right: 8px;
  background: none;
  border: none;
  color: red;
  font-size: 1.25rem;
  cursor: pointer;
}

.no-anime-message {
  color: #888;
  text-align: center;
  margin-top: 1.5rem;
}
</style>
