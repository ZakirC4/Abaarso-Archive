<template>
  <div class="archive">
    <h2 class="archive-title">Available Files</h2>

    <FileUpload @file-added="addFile" :categories="categories" />

    <label for="category-select" class="category-label">Select Category:</label>
    <select id="category-select" v-model="selectedCategory">
      <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
    </select>

    <div class="inner-boxes">
      <div v-for="item in filteredItems" :key="item.name" class="inner-box" @click="downloadFile(item.url)">
        {{ item.name }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';
import FileUpload from './FileUpload.vue';

const items = ref(JSON.parse(localStorage.getItem('items')) || [
  { name: 'Ikemen', url: 'https://drive.google.com/uc?id=1BPki8gm-Z1aBLmQ1uhKWjjmvspAd27Ek', category: 'Games' },
  { name: '7zip', url: 'https://drive.google.com/uc?id=1JQfGJk312EXb8Ruad3TLE8rP58LLI4A6', category: 'Utility' },
  { name: 'TLauncher', url: 'https://drive.google.com/uc?id=1nSbGAwb9cp3hB77eV2Elk9n5_mgFrF_G', category: 'Games' },
]);

const selectedCategory = ref('All');

const categories = ['All', 'Games', 'Utility', 'Documents', 'Music', 'Videos', 'Images', 'Books', 'Software'];

const filteredItems = computed(() => {
  return selectedCategory.value === 'All' 
    ? items.value 
    : items.value.filter(item => item.category === selectedCategory.value);
});

watch(items, (newItems) => {
  localStorage.setItem('items', JSON.stringify(newItems));
}, { deep: true });

const addFile = (newFile) => {
  if (items.value.some(item => item.name === newFile.name)) {
    alert("An item with this name already exists.");
    return;
  }
  items.value.push(newFile);
};

const downloadFile = (url) => {
  const link = document.createElement('a');
  link.href = url;
  link.download = url.split('/').pop();
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
</script>

<style scoped>
.archive {
  margin: 20px;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.archive-title {
  color: #333;
  font-size: 1.8rem;
  margin-bottom: 15px;
}

.category-label {
  color: #555;
  font-weight: bold;
  margin-bottom: 5px;
  display: block;
}

select {
  width: 100%;
  padding: 10px;
  margin-bottom: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

select:focus {
  border-color: #007bff;
}

.inner-boxes {
  display: flex;
  flex-direction: column;
}

.inner-box {
  background-color: #007bff;
  color: white;
  padding: 15px;
  margin: 5px 0;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.inner-box:hover {
  background-color: #0056b3;
  transform: translateY(-2px);
}

.inner-box:active {
  transform: translateY(0);
}
</style>
