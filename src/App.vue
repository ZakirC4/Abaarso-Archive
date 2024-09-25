<template>
  <h1 class="app-title">Abaarso Archive</h1>
  <div class="box">
    <Archive />
  </div>
</template>

<script setup>
import Archive from './components/Archive.vue';
</script>

<style scoped>
.app-title {
  text-align: center;
  color: #007BFF;
  margin: 20px 0;
  font-family: 'Helvetica Neue', Arial, sans-serif;
}

.box {
  border: 2px solid #007BFF;
  padding: 20px;
  border-radius: 8px;
  background-color: #f9f9f9;
  margin: 20px auto;
  max-width: 800px;
}
</style>
