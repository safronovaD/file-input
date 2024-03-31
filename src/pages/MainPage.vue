<template>
  <div class="main-page container">
    <FileInput :label="'Label'" :hint="'Hint text'"/>
  </div>
</template>

<script lang="ts" setup>
import FileInput from "@/components/FileInput/FileInput.vue";
</script>