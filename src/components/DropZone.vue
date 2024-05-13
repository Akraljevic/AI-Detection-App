<template>
  <div
    @dragenter.prevent="toggleActive"
    @dragleave.prevent="toggleActive"
    @dragover.prevent
    @drop.prevent="handleDrop"
    :class="{ 'active-dropzone': active }"
    class="dropzone"
  >
    <span>Dovucite</span>
    <span>ILI</span>
    <label for="dropzoneFile" class="dropzone-label">Odaberite datoteke</label>
    <input
      type="file"
      id="dropzoneFile"
      class="dropzoneFile"
      ref="fileInput"
      @change="handleFileSelection"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import { defineEmits } from "vue";

const active = ref(false);
const selectedFile = ref({});

// on change emit selected file
const emits = defineEmits(["change"])

const toggleActive = () => {
  active.value = !active.value;
};

const handleDrop = (event) => {
  toggleActive();

  const files = event.dataTransfer.files;
  readFiles(files);
};

const handleFileSelection = (event) => {
  const files = event.target.files;
  readFiles(files);
};

const readFiles = (files) => {
  Array.from(files).forEach((file) => {
    const reader = new FileReader();

    reader.onload = () => {
      if (isTextFile(file)) {
        selectedFile.value = {
          name: file.name,
          content: reader.result,
        };
        emits("change", selectedFile.value);
      }  else {
        console.log(`Unsupported file type: ${file.type}`);
      }
    };

    reader.readAsText(file);
  });
};

const isTextFile = (file) => {
  // Provjerava da li je datoteka podržanog tipa teksta (npr., .txt, .csv, .json, .xml, .html)
  // Također provjeri da li je tip datoteke programerski tekstualni
  return (
    file.type.startsWith("text") ||
    file.name.endsWith(".txt") ||
    file.name.endsWith(".csv") ||
    file.name.endsWith(".json") ||
    file.name.endsWith(".xml") ||
    file.name.endsWith(".html") ||
    file.name.endsWith(".js") ||
    file.name.endsWith(".css") ||
    file.name.endsWith(".py") ||
    file.name.endsWith(".java") ||
    file.name.endsWith(".cpp") ||
    file.name.endsWith(".c") ||
    file.name.endsWith(".h") ||
    file.name.endsWith(".hpp") ||
    file.name.endsWith(".cs") ||
    file.name.endsWith(".php") ||
    file.name.endsWith(".rb") ||
    file.name.endsWith(".sh") ||
    file.name.endsWith(".sql") ||
    file.name.endsWith(".pl") ||
    file.name.endsWith(".swift") ||
    file.name.endsWith(".kt") ||
    file.name.endsWith(".dart") ||
    file.name.endsWith(".go")
  );
}
</script>

<style scoped lang="scss">
.dropzone {
  width: 400px;
  height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 16px;
  border: 2px dashed #2f80ed;
  background-color: #0f0f1a;
  color: #fff;
  transition: 0.3s ease all;
}

span {
  font-size: 20px;
}

.dropzone-label {
  padding: 8px 12px;
  color: #fff;
  background-color: #2f80ed;
  border-radius: 4px;
  transition: 0.3s ease all;
  cursor: pointer;
}

input {
  display: none;
}

.dropzone-label:hover {
  background-color: #0056b3;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.5);
}

.active-dropzone {
  border-color: #fff;
  background-color: #2f80ed;
}

.active-dropzone .dropzone-label {
  background-color: #fff;
  color: #2f80ed;
}
</style>
