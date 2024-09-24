<script setup>
import { ref } from 'vue';

const items = ref([
  { name: 'Ikemen', url: 'https://drive.google.com/uc?id=1BPki8gm-Z1aBLmQ1uhKWjjmvspAd27Ek' },
  { name: '7zip', url: 'https://drive.google.com/uc?id=1JQfGJk312EXb8Ruad3TLE8rP58LLI4A6' },
  { name: 'TLauncher', url: 'https://drive.google.com/uc?id=1nSbGAwb9cp3hB77eV2Elk9n5_mgFrF_G' },
]);

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

<template>
  <div class="archive">
    <h2>Available Files</h2>
    <div class="inner-boxes">
      <div v-for="item in items" :key="item.name" class="inner-box" @click="downloadFile(item.url)">
        {{ item.name }}
      </div>
    </div>
  </div>
</template>

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
</style>
