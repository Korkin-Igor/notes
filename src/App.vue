<script setup>
import NoteColumn from './components/NoteColumn.vue'
import {computed, provide} from 'vue'
import {ref} from "vue";

const newNotes = ref(JSON.parse(localStorage.getItem('newNotes')) ?? [])
const inProgressNotes = ref(JSON.parse(localStorage.getItem('inProgressNotes')) ?? [])
const completedNotes = ref(JSON.parse(localStorage.getItem('completedNotes')) ?? [])
let dateTime;

const handleAddNote = () => {
  // В роли id порядковый номер заметки
  const allIds = [
    ...newNotes.value.map(n => n.id),
    ...inProgressNotes.value.map(n => n.id),
    ...completedNotes.value.map(n => n.id)
  ];

  // Если заметок нет, ID = 1, иначе берем Максимальный + 1
  const id = allIds.length === 0 ? 1 : Math.max(...allIds) + 1;

  newNotes.value.push({
    id: id,
    title: "",
    list: {
      1: { name: "", isCompleted: false },
      2: { name: "", isCompleted: false },
      3: { name: "", isCompleted: false },
    }
  })
  localStorage.setItem('newNotes', JSON.stringify(newNotes.value))
}

const addPoint = (note) => {
  const id = Object.keys(note.list).length + 1
  note.list[id] = {
    name: "",
    isCompleted: false
  }
  localStorage.setItem('newNotes', JSON.stringify(newNotes.value))
}
const removePoint = (note, id) => delete note.list[id]


const moveNote = (targetColumn, note) => {
  if (targetColumn === 'completedNotes') {
    inProgressNotes.value = inProgressNotes.value.filter(n => n.id !== note.id);
    note.completedAt = new Date().toLocaleString('ru-RU', {
      day: '2-digit',
      month: '2-digit',
      year: 'numeric',
      hour: '2-digit',
      minute: '2-digit'
    });
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
    // если заметка еще не в completedNotes
    if (!inProgressNotes.value.some(n => n.id === note.id)) {
      moveNote('inProgressNotes', note)
    }
  }

  localStorage.setItem('newNotes', JSON.stringify(newNotes.value))
  localStorage.setItem('inProgressNotes', JSON.stringify(inProgressNotes.value))
  localStorage.setItem('completedNotes', JSON.stringify(completedNotes.value))
}

const saveDataInLocalStorage = (title) => {
  switch (title) {
    case ('New'):
      localStorage.setItem('newNotes', JSON.stringify(newNotes.value))
      break;
    case ('In progress'):
      localStorage.setItem('inProgressNotes', JSON.stringify(inProgressNotes.value))
      break;
  }
}

const isProgressFull = computed(() => inProgressNotes.value.length >= 5)

provide('addPointKey', addPoint)
provide('removePointKey', removePoint)
provide('checkIsMustMovedKey', checkIsMustMoved)
provide('saveDataInLocalStorageKey', saveDataInLocalStorage)
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
      :dateTime="dateTime"
  />
</template>

<style scoped>

</style>
