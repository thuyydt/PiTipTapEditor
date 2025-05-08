<script setup lang="ts">
import { ref, computed } from 'vue'

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
import TaskList from '@tiptap/extension-task-list'
import TaskItem from '@tiptap/extension-task-item'
import Highlight from '@tiptap/extension-highlight'

// icon components
import UndoIcon from './icons/UndoIcon.vue'
import RedoIcon from './icons/RedoIcon.vue'
import CodeBlockIcon from './icons/CodeBlockIcon.vue'
import BlockQuoteIcon from './icons/BlockQuoteIcon.vue'
import BoldIcon from './icons/BoldIcon.vue'
import ItalicIcon from './icons/ItalicIcon.vue'
import StrikeIcon from './icons/StrikeIcon.vue'
import CodeIcon from './icons/CodeIcon.vue'
import UnderlineIcon from './icons/UnderlineIcon.vue'
import SuperscriptIcon from './icons/SuperscriptIcon.vue'
import SubscriptIcon from './icons/SubscriptIcon.vue'
import AlignCenterIcon from './icons/AlignCenterIcon.vue'
import AlignLeftIcon from './icons/AlignLeftIcon.vue'
import AlignJustifyIcon from './icons/AlignJustifyIcon.vue'
import AlignRightIcon from './icons/AlignRightIcon.vue'
import LightIcon from './icons/LightIcon.vue'
import DarkIcon from './icons/DarkIcon.vue'

// popover components
import PopoverHeading from './toolbar/PopoverHeading.vue'
import MobileHeadling from './toolbar/MobileHeadling.vue'
import PopoverList from './toolbar/PopoverList.vue'
import MobileList from './toolbar/MobileList.vue'
import PopoverTextColor from './toolbar/PopoverTextColor.vue'
import PopoverHighlight from './toolbar/PopoverHighlight.vue'
import PopoverLink from './toolbar/PopoverLink.vue'
import PopoverImage from './toolbar/PopoverImage.vue'

// define model
const model = defineModel({ default: "<p></p>" })

const mobileToolbar = ref<HTMLElement | null>(null)
const mode = ref('dark') // light or dark
const img = ref('')

const headingValue = ref('')
const openHeading = ref(false)

const listingValue = ref('')
const openListing = ref(false)

// click outside mobileToolbar > * to close popover
document.addEventListener('click', (event) => {
  const target = event.target as HTMLElement
  if (mobileToolbar.value && !mobileToolbar.value.contains(target)) {
    openHeading.value = false
    openListing.value = false
  }
})

const props = defineProps({
  class: {
    type: String,
    default: 'w-full h-full flex flex-col border border-neutral-200 dark:border-neutral-600 divide-y divide-neutral-200 dark:divide-neutral-500',
  },
})

// append dark mode class to props.class
const darkModeClass = computed(() => {
  return props.class + (mode.value === 'dark' ? ' dark' : '')
})

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
    FontFamily.configure({
      types: ['textStyle'],
    }),
    TextAlign.configure({
      types: ['heading', 'paragraph'],
      defaultAlignment: 'left',
      alignments: ['left', 'center', 'right', 'justify'],
    }),
    Link,
    Superscript,
    Subscript,
    Image,
    TaskItem,
    TaskList,
    Highlight,
  ],
  editorProps: {
    attributes: {
      class: 'bg-white dark:bg-neutral-600 p-4 outline-none text-neutral-900 dark:text-white focus:outline-none ring-0 h-full w-full',
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
    // check if focused to image
    const image = editor.getAttributes('image')
    if (image.src) {
      
    }
  },
  onBlur({ editor }) {
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
  if (value == 'task') {
    editor.value?.commands.toggleTaskList()
    return
  }

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
}
</script>
<template>
  <div :class="darkModeClass" class="pi-tiptap-editor relative">
    <!-- Toolbar -->
    <div 
      class="w-full absolute bottom-0 lg:top-0 lg:sticky border-t lg:border-t-0 dark:border-neutral-500 bg-white dark:bg-neutral-600 z-10 h-auto flex items-center p-1 gap-1 justify-between min-w-full pi-tiptap-editor-toolbar"
    >
      <div class="flex items-center w-fit gap-1">
        <!-- Group 1 -->
        <!-- Undo -->
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer"
          :class="{
            'text-neutral-300 dark:text-neutral-50/20': !editor?.can().undo(),
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20 text-neutral-700 dark:text-neutral-50': editor?.can().undo(),
          }"
          @click.prevent="editor?.commands.undo" 
          title="Undo"
          aria-label="Undo"
        >
          <UndoIcon class="h-5 w-5" />
        </button>
        <!-- Redo -->
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer"
          :class="{
            'text-neutral-300 dark:text-neutral-50/20': !editor?.can().redo(),
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20 text-neutral-700 dark:text-neutral-50': editor?.can().redo(),
          }"
          @click.prevent="editor?.commands.redo" 
          title="Redo"
          aria-label="Redo"
        >
          <RedoIcon class="h-5 w-5" />
        </button>
        <span class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20"></span>

        <!-- Group 2 -->
        <!-- Heading -->
        <div>
          <PopoverHeading v-model="openHeading" :value="headingValue" @edit="toogleHeading" />
        </div>
        <!-- List -->
        <div>
          <PopoverList v-model="openListing" :value="listingValue" @edit="toogleListing" />
        </div>
        <!-- Code block -->
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('codeBlock'),
            'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('codeBlock'),
          }"
          @click.prevent="editor?.commands.setNode('codeBlock')"
          title="Code block"
          aria-label="Code block"
        >
          <CodeBlockIcon class="h-5 w-5 text-neutral-700 dark:text-neutral-50" />
        </button>
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('blockQuote'),
            'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('blockQuote'),
          }"
          @click.prevent="editor?.commands.toggleBlockquote"
          title="Blockquote"
          aria-label="Blockquote"
        >
          <BlockQuoteIcon class="h-5 w-5 text-neutral-700 dark:text-neutral-50" />
        </button>
        <span class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20"></span>

        <!-- Group 3 -->
        <!-- Bold -->
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('bold'),
            'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('bold'),
          }"
          @click.prevent="editor?.commands.toggleBold" 
          title="Bold"
          aria-label="Bold"
        >
          <BoldIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
        </button>
        <!-- Italic -->
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center"
          :class="{
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('italic'),
            'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('italic'),
          }"
          @click.prevent="editor?.commands.toggleItalic" 
          title="Italic"
          aria-label="Italic"
        >
          <ItalicIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
        </button>
        <!-- Strike -->
        <button 
          type="button"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('strike'),
            'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('strike'),
          }"
          @click.prevent="editor?.commands.toggleStrike" 
          title="Strike"
          aria-label="Strike"
        >
          <StrikeIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
        </button>
        <!-- Code -->
        <button 
          type="button"
          @click.prevent="editor?.commands.toggleCode" 
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('code'),
            'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('code'),
          }"
          title="Code"
          aria-label="Code"
        >
          <CodeIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
        </button>
        <!-- Underline -->
        <button 
          type="button"
          @click.prevent="editor?.commands.toggleUnderline"
          class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
          :class="{
            'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('underline'),
            'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('underline'),
          }"
          title="Underline"
          aria-label="Underline"
        >
          <UnderlineIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
        </button>
        <!-- Text Color -->
        <div>
          <PopoverTextColor 
            :value="editor?.getAttributes('textStyle').color" 
            @edit="editor?.commands.setColor($event)" 
          />
        </div>
        <!-- Text Color -->
        <div>
          <PopoverHighlight 
            :value="editor?.getAttributes('textStyle').background" 
            @edit="editor?.commands.toggleHighlight({ color: $event })" 
          />
        </div>
        <!-- Link -->
        <div>
          <PopoverLink 
            :value="editor?.getAttributes('link').href" 
            :linked="editor?.isActive('link') && editor?.getAttributes('link').href"
            @edit="toogleLink" 
          />
        </div>
        <span class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20"></span>

        <!-- Group 4 -->
        <!-- Superscript -->
        <button 
          class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
          @click.prevent="editor?.commands.toggleSuperscript" 
          title="Superscript"
          aria-label="Superscript"
        >
          <SuperscriptIcon class="w-5 h-5" />
        </button>
        <!-- Subscript -->
        <button 
          class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
          @click.prevent="editor?.commands.toggleSubscript" 
          title="Subscript"
          aria-label="Subscript"
        >
          <SubscriptIcon class="w-5 h-5" />
        </button>
        <span class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20"></span>

        <!-- Group 5 -->
        <!-- Align left -->
        <button 
          class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
          @click.prevent="editor?.commands.toggleTextAlign('left')" 
          title="Align left"
          aria-label="Align left"
        >
          <AlignLeftIcon class="w-5 h-5" />
        </button>
        <!-- Align center -->
        <button 
          class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
          @click.prevent="editor?.commands.toggleTextAlign('center')" 
          title="Align center"
          aria-label="Align center"
        >
          <AlignCenterIcon class="w-5 h-5 " />
        </button>
        <!-- Align right -->
        <button 
          class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
          @click.prevent="editor?.commands.toggleTextAlign('right')" 
          title="Align right"
          aria-label="Align right"
        >
          <AlignRightIcon class="w-5 h-5" />
        </button>
        <!-- Align justify -->
        <button 
          class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
          @click.prevent="editor?.commands.toggleTextAlign('justify')" 
          title="Align justify"
          aria-label="Align justify"
        >
          <AlignJustifyIcon class="w-5 h-5" />
        </button>
        <span class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20"></span>

        <!-- Group 6 -->
        <div>
          <PopoverImage v-model="img" @edit="setImage" />
        </div>
      </div>

      <!-- dark/light mode -->
      <button 
        class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
        @click.prevent="toggleDarkMode" 
        title="Toggle dark/light mode"
        aria-label="Toggle dark/light mode"
      >
        <LightIcon v-if="mode === 'light'" class="w-5 h-5" />
        <DarkIcon v-else class="w-5 h-5" />
      </button>
    </div>

    <!-- Editor -->
    <EditorContent :editor="editor" class="h-full w-full overflow-auto mb-9 lg:mb-0 pi-tiptap-editor-content" />

    <div ref="mobileToolbar" class="lg:hidden">
      <MobileHeadling
        :open="openHeading"
        :value="headingValue" 
        @edit="toogleHeading"
      />
      <MobileList
        :open="openListing"
        :value="listingValue" 
        @edit="toogleListing"
      />
    </div>
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
