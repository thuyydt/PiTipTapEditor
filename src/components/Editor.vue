<script setup lang="ts">
import { ref } from 'vue'

// tiptap editor and extensions
import { useEditor, EditorContent } from '@tiptap/vue-3'
import StarterKit from '@tiptap/starter-kit'
import Underline from '@tiptap/extension-underline'
import { FontFamily } from '@tiptap/extension-font-family'
import TextAlign from '@tiptap/extension-text-align'
import TextStyle from '@tiptap/extension-text-style'
import { Color } from '@tiptap/extension-color'
import Link from '@tiptap/extension-link'
import Superscript from '@tiptap/extension-superscript'
import Subscript from '@tiptap/extension-subscript'
import Image from '@tiptap/extension-image'

// icon components
import AlignCenterIcon from './icons/AlignCenterIcon.vue'
import AlignLeftIcon from './icons/AlignLeftIcon.vue'
import AlignJustifyIcon from './icons/AlignJustifyIcon.vue'
import AlignRightIcon from './icons/AlignRightIcon.vue'

// popover components
import PopoverHeading from './toolbar/PopoverHeading.vue'
import PopoverList from './toolbar/PopoverList.vue'
import PopoverColor from './toolbar/PopoverColor.vue'
import PopoverLink from './toolbar/PopoverLink.vue'
import PopoverImage from './toolbar/PopoverImage.vue'

// tailwind icons
import { 
  ArrowUturnLeftIcon,
  ArrowUturnRightIcon,
  CodeBracketSquareIcon,
  BoldIcon,
  ItalicIcon,
  StrikethroughIcon,
  CodeBracketIcon,
  UnderlineIcon,
  MoonIcon,
  SunIcon,
} from '@heroicons/vue/24/outline'

// define model
const model = defineModel({ default: "<p></p>" })
const mode = ref('dark') // light or dark
const img = ref('')
const headingValue = ref('')
const listingValue = ref('')

const editor = useEditor({
  content: model.value,
  extensions: [
    StarterKit.configure({
      heading: {
        levels: [1, 2, 3, 4],
      },
    }),
    Underline,
    TextStyle,
    Color,
    FontFamily,
    TextAlign.configure({
      types: ['heading', 'paragraph'],
      defaultAlignment: 'left',
      alignments: ['left', 'center', 'right', 'justify'],
    }),
    Link,
    Superscript,
    Subscript,
    Image,
  ],
  editorProps: {
    attributes: {
      class: 'bg-neutral-100 text-neutral-900 p-4 outline-none focus:outline-none ring-0 h-full w-full rounded border border-neutral-200',
    },
  },
  onSelectionUpdate({ editor }) {
    // get the current heading level
    headingValue.value = editor.getAttributes('heading').level || ''

    // get the current list type
    if (editor.isActive('bulletList')) {
      listingValue.value = 'bullet'
    } else if (editor.isActive('orderedList')) {
      listingValue.value = 'number'
    } else {
      listingValue.value = ''
    }

    // get the current image src
    const image = editor.getAttributes('image')
    if (image.src) {
      img.value = image.src
    } else {
      img.value = ''
    }
  },
  onFocus({ editor }) {
    // console.log('Editor focused:', editor)

    // check if focused to image
    const image = editor.getAttributes('image')
    if (image.src) {
      
    }
  },
  onBlur({ editor }) {
    // console.log('Editor blurred:', editor)

    // get the current heading level
    headingValue.value = editor.getAttributes('heading').level || ''

    // get the current list type
    if (editor.isActive('bulletList')) {
      listingValue.value = 'bullet'
    } else if (editor.isActive('orderedList')) {
      listingValue.value = 'number'
    } else {
      listingValue.value = ''
    }

    // get the current image src
    const image = editor.getAttributes('image')
    if (image.src) {
      img.value = image.src
    } else {
      img.value = ''
    }
  },
  onCreate() {
    console.info('Custom TipTap Editor loaded!')
  },
})

const toggleDarkMode = () => {
  mode.value = mode.value === 'light' ? 'dark' : 'light'
}

const toogleHeading = (value: string) => {
  editor.value?.commands.toggleNode("heading", "paragraph", { level: value })
  // console.log('Heading changed to:', value)
}

const toogleListing = (value: string) => {
  // console.log('Listing changed to:', value)
  if (value === 'number') {
    editor.value?.commands.toggleOrderedList()
    return
  }
  
  editor.value?.commands.toggleBulletList()
}

const toogleLink = (value: string) => {
  if (value === 'unlink') {
    editor.value?.commands.unsetLink()
    return
  }

  editor.value?.commands.setLink({ href: value, target: '_blank' })
}

const setImage = () => {
  editor.value?.commands.setImage({ src: img.value, alt: 'image' })
  // console.log('Image changed to:', img.value)
}
</script>
<template>
  <div class="w-full h-full flex flex-col relative"
    :class="{ 'dark': mode === 'dark' }"
  >
    <!-- Toolbar -->
    <div class="w-full sticky top-0 bg-black/10 dark:bg-neutral-600 z-10 h-auto flex items-center flex-wrap p-1 gap-1 rounded-t justify-between">
      <div class="flex items-center flex-wrap w-fit gap-1">
        <!-- dark/light mode -->
        <button @click.prevent="toggleDarkMode" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex md:hidden items-center gap-1 min-w-8 justify-center">
          <SunIcon v-if="mode === 'light'" class="w-5 h-5 text-neutral-50" />
          <MoonIcon v-else class="w-5 h-5 text-neutral-50" />
        </button>
        <span class="md:hidden w-px h-6 bg-neutral-50/20"></span>

        <!-- Group 1 -->
        <!-- Undo -->
        <button 
          type="button"
          @click.prevent="editor?.commands.undo" 
          class="rounded-md p-1 hover:opacity-80 cursor-pointer"
          :class="{
            'hover:bg-neutral-100/20': !editor?.can().undo(),
            'bg-neutral-100/20': editor?.can().undo(),
          }"
        >
          <ArrowUturnLeftIcon class="h-5 w-5 text-neutral-50" />
        </button>
        <!-- Redo -->
        <button 
          type="button"
          @click.prevent="editor?.commands.redo" 
          class="rounded-md p-1 hover:opacity-80 cursor-pointer"
          :class="{
            'hover:bg-neutral-100/20': !editor?.can().redo(),
            'bg-neutral-100/20': editor?.can().redo(),
          }"
        >
          <ArrowUturnRightIcon class="h-5 w-5 text-neutral-50" />
        </button>
        <span class="w-px h-6 bg-neutral-50/20"></span>

        <!-- Group 2 -->
        <!-- Heading -->
        <div>
          <PopoverHeading :value="headingValue" @edit="toogleHeading" />
        </div>
        <!-- List -->
        <div>
          <PopoverList :value="listingValue" @edit="toogleListing" />
        </div>
        <!-- Code block -->
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-100/20': !editor?.isActive('codeBlock'),
            'bg-neutral-100/20': editor?.isActive('codeBlock'),
          }"
          @click.prevent="editor?.commands.setNode('codeBlock')"
        >
          <CodeBracketSquareIcon class="h-5 w-5 text-neutral-50" />
        </button>
        <span class="w-px h-6 bg-neutral-50/20"></span>

        <!-- Group 3 -->
        <!-- Bold -->
        <button 
          type="button"
          @click.prevent="editor?.commands.toggleBold" 
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-100/20': !editor?.isActive('bold'),
            'bg-neutral-100/20': editor?.isActive('bold'),
          }"
        >
          <BoldIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Italic -->
        <button 
          type="button"
          @click.prevent="editor?.commands.toggleItalic" 
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center"
          :class="{
            'hover:bg-neutral-100/20': !editor?.isActive('italic'),
            'bg-neutral-100/20': editor?.isActive('italic'),
          }"
        >
          <ItalicIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Strike -->
        <button 
          type="button"
          @click.prevent="editor?.commands.toggleStrike" 
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-100/20': !editor?.isActive('strike'),
            'bg-neutral-100/20': editor?.isActive('strike'),
          }"
        >
          <StrikethroughIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Code -->
        <button 
          type="button"
          @click.prevent="editor?.commands.toggleCode" 
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-100/20': !editor?.isActive('code'),
            'bg-neutral-100/20': editor?.isActive('code'),
          }"
        >
          <CodeBracketIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Underline -->
        <button 
          type="button"
          @click.prevent="editor?.commands.toggleUnderline"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-100/20': !editor?.isActive('underline'),
            'bg-neutral-100/20': editor?.isActive('underline'),
          }"
        >
          <UnderlineIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Highlight -->
        <div>
          <PopoverColor :value="editor?.getAttributes('textStyle').color" @edit="editor?.commands.setColor($event)" />
        </div>
        <!-- Link -->
        <div>
          <PopoverLink 
            :value="editor?.getAttributes('link').href" 
            :linked="editor?.isActive('link') && editor?.getAttributes('link').href"
            @edit="toogleLink" 
          />
        </div>
        <span class="w-px h-6 bg-neutral-50/20"></span>

        <!-- Group 4 -->
        <!-- Superscript -->
        <button @click.prevent="editor?.commands.toggleSuperscript" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center">
          <span class="text-neutral-50">x<sup>2</sup></span>
        </button>
        <!-- Subscript -->
        <button @click.prevent="editor?.commands.toggleSubscript" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center">
          <span class="text-neutral-50">x<sub>2</sub></span>
        </button>
        <span class="w-px h-6 bg-neutral-50/20"></span>

        <!-- Group 5 -->
        <!-- Align left -->
        <button @click.prevent="editor?.commands.toggleTextAlign('left')" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center">
          <AlignLeftIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Align center -->
        <button @click.prevent="editor?.commands.toggleTextAlign('center')" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center">
          <AlignCenterIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Align right -->
        <button @click.prevent="editor?.commands.toggleTextAlign('right')" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center">
          <AlignRightIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <!-- Align justify -->
        <button @click.prevent="editor?.commands.toggleTextAlign('justify')" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center">
          <AlignJustifyIcon class="w-5 h-5 text-neutral-50" />
        </button>
        <span class="w-px h-6 bg-neutral-50/20"></span>

        <!-- Group 6 -->
        <div>
          <PopoverImage v-model="img" @edit="setImage" />
        </div>
      </div>

      <!-- dark/light mode -->
      <button @click.prevent="toggleDarkMode" class="rounded-md hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer hidden md:flex items-center gap-1 min-w-8 justify-center">
        <SunIcon v-if="mode === 'light'" class="w-5 h-5 text-neutral-50" />
        <MoonIcon v-else class="w-5 h-5 text-neutral-50" />
      </button>
    </div>

    <!-- Editor -->
    <EditorContent :editor="editor" class="min-h-full w-full overflow-auto" />
  </div>
</template>

<!-- Styles -->
<style lang="scss" src="../components/styles/_variables.scss" />
<style scoped lang="scss" src="../components/styles/_editor.scss" />
<style scoped lang="scss" src="../components/styles/code-block-node.scss" />
<style scoped lang="scss" src="../components/styles/image-node.scss" />
<style scoped lang="scss" src="../components/styles/image-upload-node.scss" />
<style scoped lang="scss" src="../components/styles/list-node.scss" />
<style scoped lang="scss" src="../components/styles/paragraph-node.scss" />
