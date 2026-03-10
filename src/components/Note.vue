<script setup>
  defineProps({
    title: String,
    notes: Array
  })



  const addNote = () => {
  }
</script>

<template>
  <div class="note-card">
    <h2>{{ title }}</h2>
    <button
        class="add-note-button"
        v-if="title === 'New'"
        @click="addNote()"
    >Add note</button>
    <div class="notes" v-for="note in notes">{{ note }}</div>
  </div>
</template>

<style scoped>
.note-card {
  background: #ccc;

  padding: 10px;

  width: 30vw;
  height: 3vw;
  min-width: 200px;
  min-height: 70px;
}

.add-note-button {
  width: 40%;
  height: 40%;

  position: relative;
  left: -60px;
}

</style>