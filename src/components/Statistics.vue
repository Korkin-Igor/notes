<script setup>
import {computed} from "vue";

const props = defineProps({
  notes: Object
})

const avgTasks = computed(() => {
  // 1. Получаем массив заметок (убедитесь, что notes это массив)
  const notesArray = Array.isArray(props.notes) ? props.notes : Object.values(props.notes);

  if (notesArray.length === 0) return 0;

  // 2. Считаем общее количество пунктов (points) во всех заметках
  const totalPoints = notesArray.reduce((acc, note) => {
    // Превращаем объект list {1:..., 2:...} в массив значений и берем его длину
    const pointsCount = Object.values(note.list || {}).length;
    return acc + pointsCount;
  }, 0);

  // 3. Делим на количество заметок
  return (totalPoints / notesArray.length).toFixed(2);
});
</script>

<template>
<div class="statistics">
  <p>Среднее количество задач в карточках: {{ avgTasks }}</p>
</div>
</template>

<style scoped>
.statistics {}
</style>