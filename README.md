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
- Supports multiple languages (English, Vietnamese, Japanese)

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
- Youtube
- Image & Image Resize
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

## Requirements

```
npm install @headlessui/vue
npm install @tiptap/extension-color
npm install @tiptap/extension-font-family
npm install @tiptap/extension-highlight
npm install @tiptap/extension-image
npm install @tiptap/extension-link
npm install @tiptap/extension-subscript
npm install @tiptap/extension-superscript
npm install @tiptap/extension-task-item
npm install @tiptap/extension-task-list
npm install @tiptap/extension-text-align
npm install @tiptap/extension-typography
npm install @tiptap/extension-underline
npm install @tiptap/extension-youtube
npm install @tiptap/pm
npm install @tiptap/starter-kit
npm install @tiptap/vue-3
npm install tailwindcss
npm install vue
```

## Usage

add to main.css
```css
@custom-variant dark (&:where(.dark, .dark *));
```

in your-file.vue
```html
<template>
  <div class="[your-class]">
    <Editor 
      v-model:mode="mode" 
      v-model:content="content" 
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
<script setup>
import { ref } from 'vue'
import Editor from './components/Editor.vue'

const content = ref('<p>Hello World!</p>')
const mode = ref('dark')
</script>
```

## Demo

![\[Desktop\] (../demo/desktop.png)](https://github.com/thuyydt/mytiptap/blob/main/demo/desktop.png?raw=true)

![\[Mobile\] (../demo/mobile.png)](https://github.com/thuyydt/mytiptap/blob/main/demo/mobile.png?raw=true)
