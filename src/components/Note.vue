<script setup>
import { inject } from 'vue'

const props = defineProps({
  title: String,
  notes: Object,
  note: {
    id: Number,
    title: String,
    list: Object
  },
})

const addPoint = inject('addPointKey')
const removePoint = inject('removePointKey')
const checkIsMustMoved = inject('checkIsMustMovedKey')
const saveDataInLocalStorage = inject('saveDataInLocalStorageKey')
const isProgressFull = inject('isProgressFull')
</script>

<template>
<div class="note">
  <input
      class="note-title-input"
      type="text"
      placeholder="Note name"
      @change="saveDataInLocalStorage(title)"
      v-model="note.name"
      :disabled="title !== 'New'"
  >
  <ul v-for="(point, id) in note.list" :key="id">
    <li>
      <input
          class="is-completed"
          type="checkbox"
          @change="checkIsMustMoved(note)"
          :disabled="title === 'Completed' || point.isCompleted || (title === 'New' && isProgressFull)"
          v-model="point.isCompleted"
      >
      <input
          type="text"
          placeholder="point"
          :disabled="title !== 'New'"
          @change="saveDataInLocalStorage(title)"
          v-model="point.name">
      <button
          class="removePoint"
          @click="removePoint(note, id)"
          v-if="Object.keys(note.list).length > 3 && title==='New'"
      >Remove</button>
    </li>
  </ul>
  <button
      class="add-point"
      v-if="Object.keys(note.list).length < 5 && title==='New'"
      @click="addPoint(note)"
  >Add point</button>
  <p v-if="title === 'Completed' && note.completedAt" class="date-label">
    Завершено: {{ note.completedAt }}
  </p>
</div>
</template>

<style scoped>
  .note {
    width: 90%;
    background: #fff;
    margin: 10px;
    padding: 8px 0;
  }

  li {
    display: flex;
    justify-content: flex-start;

    min-height: 35px;

    list-style: none;
    margin-top: 5px;
    padding: 0 20px;
  }

  .note-title-input::placeholder,
  .note-title-input:focus,
  .note-title-input
  {
    font-weight: 600;
  }

  .is-completed {
    margin-right: 7px;
  }
</style>