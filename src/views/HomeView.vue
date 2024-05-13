<template>
  <div class="home">
    <header class="page-header"></header>

    <h1>Dodajte datoteku</h1>
    <DropZone @drop.prevent="drop" @change="selectedFile" />

    <div class="info-container" v-if="file_name">
      <div class="file-info">
        <span>Datoteka:</span>
        <input
          type="text"
          :value="file_name"
          readonly
          class="file-name-input"
        />
      </div>
      <div class="ai-info">
        <p>
          Vjerojatnost korištenja AI alata:
          {{ response.fake_probability.toFixed(2) }}%
        </p>
      </div>
    </div>

    <!-- Display error message if there's an error -->
    <p v-if="error" class="error-message">Nešto je pošlo po krivu</p>
  </div>
</template>

<script setup>
import DropZone from "@/components/DropZone.vue";
import { ref } from "vue";

const dropzoneFile = ref({});
const file_name = ref("");
const error = ref(false);
const response = ref({
  fake_probability: 0,
  real_probability: 0,
});
const drop = (e) => {
  dropzoneFile.value = e.dataTransfer.files[0];
};

const selectedFile = (file) => {
  console.log(file);
  // make a request to the server of url 127.0.0.1:8081 with post method and paramether of "text": file.content
  fetch("http://127.0.0.1:8000/detect-ai", {
    method: "POST",
    headers: {
      "Access-Control-Allow-Origin": "*",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ text: file.content }),
  })
    .then((response) => response.json())
    .then((data) => {
      error.value = false;
      file_name.value = file.name;
      response.value.fake_probability = data.fake_probability;
      response.value.real_probability = data.real_probability;
    })
    .catch((error) => {
      error.value = true;
    });
};
</script>

<style lang="scss" scoped>
.home {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  background-color: #1a1a2e;
  color: #fff;
  padding-top: 80px;
}

.page-header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  padding: 16px;
  text-align: center;
  background-color: #0f0f1a;
  color: #fff;
  z-index: 1000;
}

h1 {
  font-size: 40px;
  margin-bottom: 16px;
}
span {
  font-size: 17px;
  color: #fff;
}

.info-container {
  margin-top: 32px;
  padding: 20px;
  border-radius: 8px;
  background-color: #155ab4;
  width: fit-content;
}

.file-info {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
}

.file-info span {
  color: #ccc;
  margin-right: 10px;
  flex: 0 0 auto;
}

.file-name-input {
  flex: 1;
  padding: 8px;
  border: none;
  border-radius: 4px;
  background-color: #1a1a2e;
  color: #fff;
  font-size: 16px;
}

.ai-info {
  text-align: center;
}

.ai-info p {
  font-size: 18px;
  max-width: 100%;
  margin: 0 auto;
}

.error-message {
  margin-top: 16px;
  color: red;
}
</style>
