<script setup>
import { ref } from 'vue'

const MAX_FILE_SIZE = 2 * 1024 * 1024
const errorMessage = ref('')
const base64Image = ref(null)

const onFileChange = (event) => {
  const file = event.target.files[0]
  if (file) {
    if (file.size > MAX_FILE_SIZE) {
      errorMessage.value = 'File size exceeds 2MB limit.'
      base64Image.value = null
      return
    }
    const reader = new FileReader()
    reader.onloadend = () => {
      base64Image.value = reader.result
      errorMessage.value = ''
    }
    reader.readAsDataURL(file)
  }
}
</script>

<template>
  <div>
    <input type="file" @change="onFileChange" />
    <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
    <div v-if="base64Image">
      <p>Base64 Image:</p>
      <textarea :value="base64Image" rows="10" cols="50"></textarea>
      <img :src="base64Image" alt="Converted Image" />
    </div>
  </div>
</template>

<style scoped>
.error {
  color: red;
}
img {
  max-width: 100%;
  height: auto;
  display: block;
}
textarea {
  width: 100%;
  margin-bottom: 10px;
}
</style>
