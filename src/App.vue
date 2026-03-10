<script setup>
import Note from './components/Note.vue'
import {ref} from "vue";

const newNotes = ref([])
const inProgressNotes = ref([])
const completedNotes = ref([])
</script>

<template>
<Note title="New" :notes="newNotes"/>
<Note title="In progress" :notes="inProgressNotes"/>
<Note title="Completed" :notes="completedNotes"/>
</template>

<style scoped>

</style>
