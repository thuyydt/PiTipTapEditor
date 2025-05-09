<script setup lang="ts">
import { ref } from 'vue'
import Editor from './components/Editor.vue'

const mode = ref('dark')
const content = ref('<p>Hello World</p>')
const lang = ref<'en' | 'ja' | 'vi' | undefined>('en')
</script>
<template>
  <div class="w-screen h-[80vh]">
    <Editor 
      v-model:mode="mode" 
      v-model:content="content" 
      v-model:lang="lang"
      :options="{
        enableDarkMode: true,
        enableUndoRedo: true,
        enableHeading: true,
        enableList: true,
        enableCodeBlock: true,
        enableBlockquote: true,
        enableBold: true,
        enableItalic: true,
        enableStrike: true,
        enableCode: true,
        enableUnderline: true,
        enableTextColor: true,
        enableHighlight: true,
        enableLink: true,
        enableSuperscript: true,
        enableSubscript: true,
        enableTextAlign: true,
        enableYoutube: true,
        enableImage: true,
      }"
    />
  </div>
</template>