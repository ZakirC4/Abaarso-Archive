<template>
  <div class="file-upload">
    <button class="upload-button" @click="openDialog">Upload File</button>

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

        <div class="button-group">
          <button type="submit" class="submit-button">Add File</button>
          <button type="button" class="cancel-button" @click="closeDialog">Cancel</button>
        </div>
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
import { ref, defineProps, defineEmits } from 'vue';

// Define props to receive categories
const props = defineProps(['categories']);

// Define emits to send events to the parent
const emit = defineEmits(['file-added']);

const files = ref([]);
const fileName = ref('');
const fileUrl = ref('');
const fileCategory = ref('');
const dialog = ref(null);

// Open the dialog
const openDialog = () => {
  dialog.value.showModal();
};

// Close the dialog and reset the form
const closeDialog = () => {
  dialog.value.close();
  resetForm();
};

// Reset form fields
const resetForm = () => {
  fileName.value = '';
  fileUrl.value = '';
  fileCategory.value = props.categories[0];
};

// Emit the new file to the parent component
const addFile = () => {
  if (!fileName.value || !fileUrl.value || !fileCategory.value) return;

  const newFile = {
    name: fileName.value,
    url: fileUrl.value,
    category: fileCategory.value,
  };

  emit('file-added', newFile);
  closeDialog();
};
</script>

<style scoped>
.file-upload {
  margin: 20px;
  font-family: 'Arial', sans-serif;
}

.upload-button {
  padding: 12px 20px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.upload-button:hover {
  background-color: #0056b3;
}

.dialog {
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.file-list {
  margin-top: 20px;
}

.file-item {
  background-color: #f0f4ff;
  border: 1px solid #cce0ff;
  padding: 12px;
  margin: 5px 0;
  border-radius: 5px;
  transition: transform 0.2s;
}

.file-item:hover {
  transform: scale(1.02);
}

.button-group {
  display: flex;
  gap: 10px;
  margin-top: 15px;
}

.submit-button,
.cancel-button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.submit-button {
  background-color: #28a745;
  color: white;
}

.submit-button:hover {
  background-color: #218838;
}

.cancel-button {
  background-color: #dc3545;
  color: white;
}

.cancel-button:hover {
  background-color: #c82333;
}

label {
  margin-top: 10px;
  display: block;
  font-weight: bold;
}

input,
select {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #ced4da;
  border-radius: 5px;
}
</style>
