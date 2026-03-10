<script setup>
import NoteColumn from './components/NoteColumn.vue'
import {computed, provide} from 'vue'
import {ref} from "vue";

const newNotes = ref(JSON.parse(localStorage.getItem('newNotes')) ?? [])
const inProgressNotes = ref(localStorage.getItem('inProgressNotes') ?? [])
const completedNotes = ref(localStorage.getItem('completedNotes') ?? [])

const handleAddNote = () => {
  newNotes.value.push({
    // В роли id просто порядковый номер
    id: (newNotes.value.length + inProgressNotes.value.length + completedNotes.value.length ?? 0) + 1,
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
  localStorage.setItem('newNotes', JSON.stringify(newNotes.value))
}

const addPoint = (targetColumn, note) => {
  const id = Object.keys(note.list).length + 1
  note.list[id] = {
    name: "",
    isCompleted: false
  }
  localStorage.setItem(`${targetColumn}`, JSON.stringify(targetColumn))
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
const checkIsMustMoved = (note) => {
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

  localStorage.setItem('newNotes', JSON.stringify(newNotes.value))
  localStorage.setItem('inProgressNotes', JSON.stringify(inProgressNotes.value))
  localStorage.setItem('completedNotes', JSON.stringify(completedNotes.value))
}

const isProgressFull = computed(() => inProgressNotes.value.length >= 5)

provide('addPointKey', addPoint)
provide('removePointKey', removePoint)
provide('checkIsMustMovedKey', checkIsMustMoved)
provide('isProgressFull', isProgressFull)
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
