<template>
    <div class="file-upload">
      <button @click="openDialog">Upload File</button>
      
      <dialog ref="dialog" class="dialog">
        <form @submit.prevent="addFile">
          <h3>Upload a New File</h3>
          
          <label for="file-name">File Name:</label>
          <input type="text" v-model="fileName" id="file-name" required />
  
          <label for="file-url">File URL:</label>
          <input type="url" v-model="fileUrl" id="file-url" required />
  
          <label for="file-category">Category:</label>
          <select v-model="fileCategory" id="file-category" required>
            <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
          </select>
  
          <button type="submit">Add File</button>
          <button type="button" @click="closeDialog">Cancel</button>
        </form>
      </dialog>
  
      <div class="file-list">
        <h2>Uploaded Files</h2>
        <div v-for="item in files" :key="item.name" class="file-item">
          {{ item.name }} - {{ item.category }}
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const files = ref([]);
  const fileName = ref('');
  const fileUrl = ref('');
  const fileCategory = ref('');
  const dialog = ref(null);
  
  // Define available categories
  const categories = ['Games', 'Utility', 'Documents', 'Music', 'Videos'];
  
  // Open the dialog
  const openDialog = () => {
    dialog.value.showModal();
  };
  
  // Close the dialog
  const closeDialog = () => {
    dialog.value.close();
    resetForm();
  };
  
  // Reset form fields
  const resetForm = () => {
    fileName.value = '';
    fileUrl.value = '';
    fileCategory.value = categories[0];
  };
  
  // Add new file
  const addFile = () => {
    files.value.push({
      name: fileName.value,
      url: fileUrl.value,
      category: fileCategory.value,
    });
    closeDialog();
  };
  </script>
  
  <style scoped>
  .file-upload {
    margin: 20px 0;
  }
  
  .dialog {
    padding: 20px;
  }
  
  .file-list {
    margin-top: 20px;
  }
  
  .file-item {
    background-color: #e7f3ff;
    border: 1px solid #c1e1ff;
    padding: 10px;
    margin: 5px 0;
    border-radius: 5px;
  }
  
  button {
    margin-top: 10px;
    padding: 10px 15px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  button:hover {
    background-color: #218838;
  }
  
  label {
    margin-top: 10px;
    display: block;
  }
  </style>
  