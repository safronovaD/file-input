<template>
  <div class="file-input">
    <div class="file-input__label">{{ label }}</div>
    <div class="file-input__wrapper">
      <Button v-if="!fileName" :text="'Выбрать файл'" :disabled="disabled" class="file-input__button" @click="generateClickEvent"/>
      <Button v-else-if="isFileUploading" :text="'Отменить'" class="file-input__button" @click="resetFileInput"/>
      <Button v-else :text="'Удалить'" class="file-input__button" @click="deleteFile"/>
      <img v-if="isFileUploading" src="@/assets/images/circular-progress-indicator.svg" class="file-input__loader" alt="Загрузка"/>
      <label for="file" class="file-input__name" @click="onLabelClick">{{ labelText }}</label>
      <input id="file" type="file" name="file" class="file-input__input" ref="fileInput" @change="uploadFile">
    </div>
    <div v-if="showError" class="file-input__error">{{ errorMessage }}</div>
    <div v-else class="file-input__hint">{{ hint }}</div>
  </div>
</template>

<script lang="ts" setup>
import Button from "@/components/Button/Button.vue";
import {computed, defineEmits, defineProps, ref} from "vue";

const props = defineProps({
  label: {
    type: String,
    required: true
  },
  hint: {
    type: String,
    required: true
  },
  errorMessage: {
    type: String,
    default: 'Error message'
  },
  disabled: {
    type: Boolean,
    default: false
  }
});

const emits = defineEmits(['fileUploaded', 'fileDeleted']);

const reader = new FileReader();

let fileInput = ref(),
    fileName = ref<String>(''),
    isFileUploading = ref<boolean>(false),
    showError = ref<boolean>(false);

const labelText = computed(() => {
  return fileName.value ? fileName.value : 'Файл не выбран';
});

function generateClickEvent(): void {
  const event = new MouseEvent('click');
  fileInput.value.dispatchEvent(event);
}

function uploadFile(event: Event): void {
  isFileUploading.value = true;
  showError.value = false;

  const target = event.target as HTMLInputElement;
  const file: File = (target.files as FileList)[0];
  fileName.value = file.name;

  reader.onload = () => {
    isFileUploading.value = false;
    emits('fileUploaded', reader.result);
  };
  reader.onerror = () => {
    isFileUploading.value = false;
    showError.value = true;
    resetFileInput();
  };

  reader.readAsDataURL(file);
}

function resetFileInput(): void {
  reader.abort();
  isFileUploading.value = false;
  fileInput.value.value = '';
  fileName.value = '';
}

function deleteFile(): void {
  resetFileInput();
  emits('fileDeleted');
}

function onLabelClick(event: Event): void {
  if(props.disabled) {
    event.preventDefault();
  }
}
</script>

<style scoped>
.file-input__label {
  font-weight: 500;
  font-size: 13px;
  line-height: 120%;
  color: var(--color-neutral-900);
  margin-bottom: 12px;
}

.file-input__input {
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0;
}

.file-input__wrapper {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.file-input__button {
  margin-right: 16px;
}

.file-input__button:focus {
  outline: none;
  box-shadow: 0 0 0 2px rgba(107, 114, 128, 0.1);
}

.file-input__loader {
  margin-right: 8px;
  animation: rotation 2s infinite linear;
}

.file-input__name {
  font-size: 14px;
  line-height: 114%;
  letter-spacing: -0.01em;
  color: var(--color-neutral-400);
}

.file-input__error {
  font-size: 13px;
  line-height: 120%;
  color: var(--color-error-500);
}

.file-input__hint {
  font-size: 13px;
  line-height: 120%;
  color: var(--color-neutral-500);
}
</style>