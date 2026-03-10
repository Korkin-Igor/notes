<script setup>
import NoteColumn from './components/NoteColumn.vue'
import {ref} from "vue";

const handleAddNote = (columnName) => {
  switch (columnName) {
    case "New":
      newNotes.value.push({
        title: "Note name",
        list: ["Point", "Point", "Point"]
      })
      break
    case "In progress":
      break
    case "Completed":
      break
  }
}

const newNotes = ref([])
const inProgressNotes = ref([])
const completedNotes = ref([])

</script>

<template>
<NoteColumn
    title="New"
    :notes="newNotes"
    @addNote="handleAddNote"
/>
<NoteColumn
    title="In progress"
    :notes="inProgressNotes"
    @addNote="handleAddNote"
/>
<NoteColumn
    title="Completed"
    :notes="completedNotes"
    @addNote="handleAddNote"
/>
</template>

<style scoped>

</style>
