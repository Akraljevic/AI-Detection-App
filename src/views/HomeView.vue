<template>
  <div class="home">
    <header class="page-header"></header>

    <h1>Dodajte datoteku</h1>
    <DropZone @drop.prevent="drop" @change="selectedFile" />
    <span v-if="file_name" class="file-info"
      >Datoteka: {{ file_name }}</span
    >
    <p v-if="error" style="color: red;">Nešto je pošlo po krivu</p>
    <div v-else-if="file_name">
      <p>Vjerovatnost korištenja AI alata: {{ response.fake_probability.toFixed(2) }}%</p>
    </div>
  </div>
</template>

<script setup
>
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
      'Access-Control-Allow-Origin': '*',
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
  justify-content: center;
  align-items: center;
  background-color: #1a1a2e;
  color: #fff;
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
  margin-bottom: 32px;
}

.file-info {
  margin-top: 32px;
  color: #ccc;
}
</style>
