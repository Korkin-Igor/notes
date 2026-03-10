<script setup>
  const props = defineProps({
    note: {
      title: String,
      list: Array
    }
  })


</script>

<template>
<div class="note">
  <input class="note-title-input" type="text" :placeholder=note.title>
  <ul v-for="point in note.list">
    <li>
      <input type="text" placeholder="point">
    </li>
  </ul>
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
    list-style: none;
    margin-top: 5px;
  }

  .note-title-input::placeholder,
  .note-title-input:focus
  { font-weight: 600; }
</style>