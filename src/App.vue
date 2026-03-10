<script setup>
import NoteColumn from './components/NoteColumn.vue'
import { provide } from 'vue'
import {ref} from "vue";

const newNotes = ref([])
const inProgressNotes = ref([])
const completedNotes = ref([])

const handleAddNote = () => {
  newNotes.value.push({
    id: (newNotes.value.length ?? 0) + 1,
    title: "",
    list: {
      1: {
        name: "",
        isCompleted: false
      },
      2: {
        name: "",
        isCompleted: false
      },
      3: {
        name: "",
        isCompleted: false
      },
    }
  })
}

const addPoint = (note) => {
  const id = Object.keys(note.list).length + 1
  note.list[id] = {
    name: "",
    isCompleted: false
  }
}
const removePoint = (note, id) => delete note.list[id]


const moveNote = (targetColumn, note) => {
  if (targetColumn === 'completedNotes') {
    inProgressNotes.value = inProgressNotes.value.filter(n => n.id !== note.id);
    completedNotes.value.push(note)
  } else if (targetColumn === 'inProgressNotes') {
    newNotes.value = newNotes.value.filter(n => n.id !== note.id);
    inProgressNotes.value.push(note)
  }
}
const switchIsCompleted = (note, point) => {
  point.isCompleted = !point.isCompleted

  // Превращаем объект в массив значений, чтобы использовать методы массива
  const pointsArray = Object.values(note.list)
  const total = pointsArray.length

  if (total === 0) return

  const completedCount = pointsArray.filter(p => p.isCompleted).length
  const ratio = completedCount / total

  if (ratio === 1) {
    moveNote('completedNotes', note)
  } else if (ratio > 0.5) {
    moveNote('inProgressNotes', note)
  }
}

provide('addPointKey', addPoint)
provide('removePointKey', removePoint)
provide('switchIsCompletedKey', switchIsCompleted)
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
  />
  <NoteColumn
      title="Completed"
      :notes="completedNotes"
  />
</template>

<style scoped>

</style>
