<template>
  <div class="home">
    <header class="page-header"></header>

    <h1>Dodajte datoteku</h1>
    <DropZone @drop.prevent="drop" @change="selectedFile" />
    <span v-if="dropzoneFile" class="file-info"
      >Datoteka: {{ dropzoneFile.name }}</span
    >
  </div>
</template>

<script>
import DropZone from "@/components/DropZone.vue";
import { ref } from "vue";

export default {
  name: "HomeView",
  components: {
    DropZone,
  },
  setup() {
    let dropzoneFile = ref({});

    const drop = (e) => {
      dropzoneFile.value = e.dataTransfer.files[0];
    };

    const selectedFile = () => {
      const files = document.querySelector(".dropzoneFile").files;
      if (files.length > 0) {
        dropzoneFile.value = files[0];
      }
    };

    return { dropzoneFile, drop, selectedFile };
  },
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
