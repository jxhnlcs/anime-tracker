<template>
  <form @submit.prevent="$emit('search')">
    <input placeholder="Pesquise por um anime..." :value="query" @input="updateQuery" type="text"/>
    <button class="button" type="submit">Pesquisar</button>
  </form>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  query: String,
});

const emit = defineEmits(["update:query", "search"]);

const updateQuery = (event) => {
  emit("update:query", event.target.value);
};
</script>

<style scoped>
form {
  display: flex;
  max-width: 480px;
  margin: 0 auto 1.5rem;
}

form input {
  appearance: none;
  outline: none;
  border: none;
  background: white;

  display: block;
  color: #888;
  font-size: 1.125rem;
  padding: 0.5rem 1rem;
  width: 100%;
}

.button {
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

.button:hover {
  background-position: right;
}
</style>
