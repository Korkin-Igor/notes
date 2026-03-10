<script setup>
import { inject } from 'vue'

const props = defineProps({
  title: String,
  note: {
    id: Number,
    title: String,
    list: Object
  }
})

const addPoint = inject('addPointKey')
const removePoint = inject('removePointKey')
const switchIsCompleted = inject('switchIsCompletedKey')
console.log(props.title)
</script>

<template>
<div class="note">
  <input class="note-title-input" type="text" placeholder="Note name">
  <ul v-for="(point, id) in note.list" :key="id">
    <li>
      <input
          class="is-completed"
          type="checkbox"
          @click="switchIsCompleted(note, point)"
          :disabled="title === 'Completed'"
      >
      <input type="text" placeholder="point">
      <button
          class="removePoint"
          @click="removePoint(note, id)"
          v-if="Object.keys(note.list).length > 3"
      >Remove</button>
    </li>
  </ul>
  <button
      class="add-point"
      v-if="Object.keys(note.list).length < 5"
      @click="addPoint(note)"
  >Add point</button>
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