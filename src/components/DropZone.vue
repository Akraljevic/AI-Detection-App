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
      multiple
    />
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "DropZone",
  setup() {
    const active = ref(false);

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
            console.log(`File name: ${file.name}`);
            console.log(`File content: ${reader.result}`);
          } else if (isSupportedDocument(file)) {
            console.log(`File name: ${file.name}`);
            console.log(`Document format: ${file.type}`);
            // U konzoli ispisano ime i format dokumenta koji je priložen
          } else {
            console.log(`Unsupported file type: ${file.type}`);
          }
        };

        reader.readAsText(file);
      });
    };

    const isTextFile = (file) => {
      // Provjerava da li je datoteka podržanog tipa teksta (npr., .txt, .csv, .json, .xml, .html)
      return (
        file.type.startsWith("text") ||
        file.name.endsWith(".txt") ||
        file.name.endsWith(".csv") ||
        file.name.endsWith(".json") ||
        file.name.endsWith(".xml") ||
        file.name.endsWith(".html")
      );
    };

    const isSupportedDocument = (file) => {
      // Provjerava je li dokument podržanog formata (npr. Word, PDF)
      return (
        file.type === "application/pdf" ||
        file.type ===
          "application/vnd.openxmlformats-officedocument.wordprocessingml.document" ||
        file.type === "application/msword" ||
        file.type === "application/vnd.ms-excel" ||
        file.name.endsWith(".docx") ||
        file.name.endsWith(".pdf") ||
        file.name.endsWith(".xls") ||
        file.name.endsWith(".ppt")
      );
    };

    return { active, toggleActive, handleDrop, handleFileSelection };
  },
};
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
