<script setup lang="ts">
const props = defineProps({
  class: {
    type: String,
    default: ''
  },
})
</script>
<template>
  <svg :class="props.class" class="pi-tiptap-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 -960 960 960" width="24px"><path d="M80 0v-160h800V0H80Zm140-280 210-560h100l210 560h-96l-50-144H368l-52 144h-96Zm176-224h168l-82-232h-4l-82 232Z"/></svg>
</template>