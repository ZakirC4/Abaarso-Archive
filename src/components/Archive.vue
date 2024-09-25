<template>
  <div class="archive">
    <h2 style="color: black;">Available Files</h2>
    
    <FileUpload @file-added="addFile" />

    <label for="category-select" style="color: #333;">Select Category:</label>
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
import { ref, computed } from 'vue';
import FileUpload from './FileUpload.vue';

const items = ref([
  { name: 'Ikemen', url: 'https://drive.google.com/uc?id=1BPki8gm-Z1aBLmQ1uhKWjjmvspAd27Ek', category: 'Games' },
  { name: '7zip', url: 'https://drive.google.com/uc?id=1JQfGJk312EXb8Ruad3TLE8rP58LLI4A6', category: 'Utility' },
  { name: 'TLauncher', url: 'https://drive.google.com/uc?id=1nSbGAwb9cp3hB77eV2Elk9n5_mgFrF_G', category: 'Games' },
]);

const selectedCategory = ref('All');

const categories = computed(() => {
  const uniqueCategories = new Set(items.value.map(item => item.category));
  return ['All', ...uniqueCategories];
});

const filteredItems = computed(() => {
  if (selectedCategory.value === 'All') {
    return items.value;
  }
  return items.value.filter(item => item.category === selectedCategory.value);
});

// Add a new file to the items list
const addFile = (newFile) => {
  items.value.push(newFile);
};

const downloadFile = (url) => {
  console.log('Downloading file from:', url);
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
  margin: 20px 0;
}

.inner-boxes {
  display: flex;
  flex-direction: column;
  margin-top: 10px;
}

.inner-box {
  background-color: #28a745;
  color: white;
  padding: 15px;
  margin: 5px 0;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.inner-box:hover {
  background-color: #218838;
}

label {
  margin-bottom: 10px;
  display: block;
}

</style>
