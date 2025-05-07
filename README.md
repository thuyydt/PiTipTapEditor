# mytiptap

Simple Tiptap Editor with Vue 3 and Tailwind CSS.

## Features

- Simple and clean design
- Easy to use
- Customizable
- Supports Tailwind CSS
- Supports Vue 3
- Supports dark mode
- Supports custom themes
- Supports custom fonts
- Supports custom colors
- Supports custom styles

# Toolbar

- Dark mode toggle
- Undo/Redo
- Heading
- List
- Blockcode
- Bold
- Italic
- Underline
- Strike
- Code
- Text color
- Link
- Image
- Subscript
- Superscript
- Text align

## Installation

```bash
npm install 
```

```bash
npm run dev
```

## Usage

```html
<template>
  <div class="[your-class]">
    <Editor v-model="content" />
  </div>
</template>
<script setup>
import { ref } from 'vue'
import Editor from './components/Editor.vue'

const content = ref('<p>Hello World!</p>')
</script>
```

