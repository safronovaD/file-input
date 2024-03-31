<template>
  <button :class="['button', {'button--disabled': disabled}]" :disabled="disabled">{{ text }}</button>
</template>

<script lang="ts" setup>
import {defineProps} from "vue";

const props = defineProps({
  text: {
    type: String,
    required: true
  },
  disabled: {
    type: Boolean,
    default: false
  }
});
</script>

<style scoped>
.button {
  width: fit-content;
  padding: 12px;
  background-color: var(--color-base-white);
  border: 1px solid var(--color-neutral-200);
  border-radius: 12px;
  font-size: 14px;
  line-height: 114%;
  letter-spacing: -0.01em;
  color: var(--color-neutral-700);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.02), 0 3px 7px -3px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  transition: all .3s;
}

.button:hover:not(.button--disabled) {
  border-color: var(--color-neutral-300);
  background-color: var(--color-neutral-100);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.02);
}

.button:active:not(.button--disabled) {
  border-color: var(--color-neutral-300);
  background-color: var(--color-neutral-200);
  box-shadow: none;
}

.button--disabled {
  color: var(--color-neutral-400);
  cursor: auto;
}
</style>