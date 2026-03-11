<script setup>
import Note from "./Note.vue"
import Statistics from "./Statistics.vue";
defineProps({
  title: String,
  notes: Object
})

const emit = defineEmits(['addNote'])

const addNote = () => emit('addNote')


</script>

<template>
  <div class="note-column">
    <h2>{{ title }}</h2>
    <button
        class="add-note-button"
        v-if="title === 'New' && notes.length < 3"
        @click="addNote()"
    >Add note</button>
    <div class="notes" v-for="note in notes" :key="note.id">
      <Note :notes="notes" :note="note" :title="title"></Note>
    </div>
    <Statistics :notes="notes" v-if="title === 'Completed' && Object.keys(notes).length >= 5"></Statistics>
    <p v-else-if="title === 'Completed'">Для статистики необходимо завершить минимум 5 карточек</p>
  </div>
</template>

<style scoped>
.note-column {
  background: #ccc;

  padding: 10px;

  min-width: 350px;
}

.add-note-button {
  width: 40%;
}

</style>