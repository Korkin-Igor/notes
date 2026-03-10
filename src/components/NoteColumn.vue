<script setup>
import Note from "./Note.vue"
defineProps({
  title: String,
  notes: Array
})

const emit = defineEmits(['addNote'])

const addNote = () => emit('addNote')


</script>

<template>
  <div class="note-column">
    <h2>{{ title }}</h2>
    <button
        class="add-note-button"
        v-if="title === 'New'"
        @click="addNote()"
    >Add note</button>
    <div class="notes" v-for="note in notes">
      <Note :note="note" :title="title"></Note>
    </div>
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