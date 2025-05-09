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
import TaskList from '@tiptap/extension-task-list'
import TaskItem from '@tiptap/extension-task-item'
import Highlight from '@tiptap/extension-highlight'
import ImageResize from './extension/image-resize'
import Youtube from '@tiptap/extension-youtube'

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
import MobileTextColor from './toolbar/MobileTextColor.vue'
import PopoverHighlight from './toolbar/PopoverHighlight.vue'
import MobileHighlight from './toolbar/MobileHighlight.vue'
import PopoverLink from './toolbar/PopoverLink.vue'
import MobileLink from './toolbar/MobileLink.vue'
import PopoverYoutube from './toolbar/PopoverYoutube.vue'
import MobileYoutube from './toolbar/MobileYoutube.vue'
import PopoverImage from './toolbar/PopoverImage.vue'
import MobileImage from './toolbar/MobileImage.vue'

// define model
const content = defineModel('content', { default: "<p></p>" })
const mode = defineModel('mode', { type: String, default: 'dark' }) // light or dark

import type { PropType } from 'vue'
const lang = defineModel<'ja' | 'en' | 'vi'>('lang', { type: String as PropType<'ja' | 'en' | 'vi'>, default: 'en' }) // language
import ja from './lang/ja.json'
import en from './lang/en.json'
import vi from './lang/vi.json'
// register i18n
const trans = {
  ja,
  en,
  vi,
}

const mobileToolbar = ref<HTMLElement | null>(null)
const video = ref('') // youtube video id
const img = ref('') // image src

const listingValue = ref('') // bullet, number, task
const headingValue = ref('') // 1, 2, 3, 4
const openHeading = ref(false) // mobile heading
const openListing = ref(false) // mobile listing
const openTextColor = ref(false) // mobile text color
const openHighlight = ref(false) // mobile highlight
const openLink = ref(false) // mobile link
const openYoutube = ref(false) // mobile youtube
const openImage = ref(false) // mobile image

const props = defineProps({
  class: {
    type: String,
    default: 'w-full h-full flex flex-col border border-neutral-200 dark:border-neutral-600 divide-y divide-neutral-200 dark:divide-neutral-500',
  },
  options: {
    type: Object,
    default: () => ({
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
    }),
  }
})

// append dark mode class to props.class
const darkModeClass = computed(() => {
  return props.class + (mode.value === 'dark' ? ' dark' : '')
})

const showtoolbar = computed(() => {
  return props.options.enableDarkMode || 
         props.options.enableUndoRedo || 
         props.options.enableHeading || 
         props.options.enableList || 
         props.options.enableCodeBlock || 
         props.options.enableBlockquote || 
         props.options.enableBold || 
         props.options.enableItalic || 
         props.options.enableStrike || 
         props.options.enableCode || 
         props.options.enableUnderline || 
         props.options.enableTextColor || 
         props.options.enableHighlight || 
         props.options.enableLink || 
         props.options.enableSuperscript || 
         props.options.enableSubscript ||
         props.options.enableTextAlign ||
         props.options.enableYoutube ||
         props.options.enableImage
})

const editor = useEditor({
  content: content.value,
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
    Link.configure({
      protocols: ['http', 'https', 'mailto'],
    }),
    Superscript,
    Subscript,
    ImageResize,
    TaskItem,
    TaskList,
    Highlight.configure({
      multicolor: true,
    }),
    Youtube.configure({
      nocookie: true,
    }),
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
    img.value = image.src || ''

    // get the current video src
    const video = editor.getAttributes('youtube')
    video.value = video.src || ''
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
    img.value = image.src || ''

    // get the current video src
    const video = editor.getAttributes('youtube')
    video.value = video.src || ''
  },
  onCreate() {
    console.group('PiTipTap Editor loaded!')
    console.info('Version: 1.0.0')
    console.info('Author: Phinq')
    console.info('License: MIT')
    console.groupEnd()
  },
})

const toggleDarkMode = () => {
  mode.value = mode.value === 'light' ? 'dark' : 'light'
}

const toogleHeading = (value: string) => {
  editor.value?.commands.toggleNode("heading", "paragraph", { level: value })
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
  editor.value?.commands.setImage({
    src: img.value,
  })
}

const setVideo = () => {
  editor.value?.commands.setYoutubeVideo({
    src: video.value,
    width: 640,
    height: 480,
  })
}

const toogleHighlight = (value: string) => {
  if (!value) {
    editor.value?.commands.unsetHighlight()
    return
  }

  editor.value?.commands.toggleHighlight({ color: value })
}

// click outside mobileToolbar > * to close popover
document.addEventListener('click', (event) => {
  const target = event.target as HTMLElement
  if (!mobileToolbar.value) return
  if (!mobileToolbar.value.contains(target)) {
    openHeading.value = false
    openListing.value = false
    openTextColor.value = false
    openHighlight.value = false
    openLink.value = false
    openYoutube.value = false
    openImage.value = false
  }
})
</script>
<template>
  <div :class="darkModeClass" class="pi-tiptap-editor relative">
    <!-- Toolbar -->
    <div v-if="showtoolbar"
      class="w-full absolute bottom-0 lg:top-0 lg:sticky border-t lg:border-t-0 dark:border-neutral-500 bg-white/80 dark:bg-neutral-600 z-10 h-auto flex items-center p-1 gap-1 justify-between min-w-full pi-tiptap-editor-toolbar backdrop-filter backdrop-blur transition duration-200 ease-in-out delay-75"
    >
      <div class="flex items-center w-fit gap-1">
        <!-- Group 1 -->
        <template v-if="props.options.enableUndoRedo">
          <!-- Undo -->
          <button 
            type="button"
            class="rounded-md p-1 hover:opacity-80 cursor-pointer"
            :class="{
              'text-neutral-300 dark:text-neutral-50/20': !editor?.can().undo(),
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20 text-neutral-700 dark:text-neutral-50': editor?.can().undo(),
            }"
            @click.prevent="editor?.commands.undo" 
            :title="trans[lang].undo"
            :aria-label="trans[lang].undo"
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
            :title="trans[lang].redo"
            :aria-label="trans[lang].redo"
          >
            <RedoIcon class="h-5 w-5" />
          </button>
          <span class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20 pi-tiptap-divide-1"></span>
        </template>

        <!-- Group 2 -->
        <!-- Heading -->
        <div v-if="props.options.enableHeading">
          <PopoverHeading 
            v-model:heading="openHeading" 
            v-model:listing="openListing"
            v-model:textcolor="openTextColor"
            v-model:highlight="openHighlight"
            v-model:link="openLink"
            v-model:youtube="openYoutube"
            v-model:image="openImage"
            :lang="lang"
            :value="headingValue" 
            @edit="toogleHeading" 
          />
        </div>
        <!-- List -->
        <div v-if="props.options.enableList">
          <PopoverList 
            v-model:heading="openHeading" 
            v-model:listing="openListing"
            v-model:textcolor="openTextColor"
            v-model:highlight="openHighlight"
            v-model:link="openLink"
            v-model:youtube="openYoutube"
            v-model:image="openImage"
            :lang="lang"
            :value="listingValue" 
            @edit="toogleListing" 
          />
        </div>
        <!-- Code block -->
        <div v-if="props.options.enableCodeBlock">
          <button 
            type="button"
            class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
            :class="{
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('codeBlock'),
              'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('codeBlock'),
            }"
            @click.prevent="editor?.commands.setNode('codeBlock')"
            :title="trans[lang].codeBlock"
            :aria-label="trans[lang].codeBlock"
          >
            <CodeBlockIcon class="h-5 w-5 text-neutral-700 dark:text-neutral-50" />
          </button>
        </div>
        <div v-if="props.options.enableBlockquote">
          <button 
            type="button"
            class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
            :class="{
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('blockQuote'),
              'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('blockQuote'),
            }"
            @click.prevent="editor?.commands.toggleBlockquote"
            :title="trans[lang].blockQuote"
            :aria-label="trans[lang].blockQuote"
          >
            <BlockQuoteIcon class="h-5 w-5 text-neutral-700 dark:text-neutral-50" />
          </button>
        </div>
        <span 
          v-if="props.options.enableHeading || props.options.enableList || props.options.enableCodeBlock || props.options.enableBlockquote"
          class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20 pi-tiptap-divide-2"
        ></span>

        <!-- Group 3 -->
        <div v-if="props.options.enableBold">
          <!-- Bold -->
          <button 
            type="button"
            class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
            :class="{
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('bold'),
              'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('bold'),
            }"
            @click.prevent="editor?.commands.toggleBold" 
            :title="trans[lang].bold"
            :aria-label="trans[lang].bold"
          >
            <BoldIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
          </button>
        </div>
        <div v-if="props.options.enableItalic">
          <!-- Italic -->
          <button 
            type="button"
            class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center"
            :class="{
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('italic'),
              'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('italic'),
            }"
            @click.prevent="editor?.commands.toggleItalic" 
            :title="trans[lang].italic"
            :aria-label="trans[lang].italic"
          >
            <ItalicIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
          </button>
        </div>
        <div v-if="props.options.enableStrike">
          <!-- Strike -->
          <button 
            type="button"
            class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
            :class="{
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('strike'),
              'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('strike'),
            }"
            @click.prevent="editor?.commands.toggleStrike" 
            :title="trans[lang].strike"
            :aria-label="trans[lang].strike"
          >
            <StrikeIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
          </button>
        </div>
        <div v-if="props.options.enableCode">
          <!-- Code -->
          <button 
            type="button"
            @click.prevent="editor?.commands.toggleCode" 
            class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
            :class="{
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('code'),
              'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('code'),
            }"
            :title="trans[lang].code"
            :aria-label="trans[lang].code"
          >
            <CodeIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
          </button>
        </div>
        <div v-if="props.options.enableUnderline">
          <!-- Underline -->
          <button 
            type="button"
            @click.prevent="editor?.commands.toggleUnderline"
            class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1"
            :class="{
              'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !editor?.isActive('underline'),
              'bg-neutral-200 dark:bg-neutral-100/20': editor?.isActive('underline'),
            }"
            :title="trans[lang].underline"
            :aria-label="trans[lang].underline"
          >
            <UnderlineIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50" />
          </button>
        </div>
        <!-- Text Color -->
        <div v-if="props.options.enableTextColor"> 
          <PopoverTextColor 
            v-model:heading="openHeading" 
            v-model:listing="openListing"
            v-model:textcolor="openTextColor"
            v-model:highlight="openHighlight"
            v-model:link="openLink"
            v-model:youtube="openYoutube"
            v-model:image="openImage"
            :lang="lang"
            :value="editor?.getAttributes('textStyle').color" 
            @edit="editor?.commands.setColor($event)" 
          />
        </div>
        <!-- Highlight -->
        <div v-if="props.options.enableHighlight">
          <PopoverHighlight 
            v-model:heading="openHeading" 
            v-model:listing="openListing"
            v-model:textcolor="openTextColor"
            v-model:highlight="openHighlight"
            v-model:link="openLink"
            v-model:youtube="openYoutube"
            v-model:image="openImage"
            :lang="lang"
            :value="editor?.getAttributes('textStyle').background" 
            @edit="toogleHighlight" 
          />
        </div>
        <!-- Link -->
        <div v-if="props.options.enableLink">
          <PopoverLink 
            v-model:heading="openHeading" 
            v-model:listing="openListing"
            v-model:textcolor="openTextColor"
            v-model:highlight="openHighlight"
            v-model:link="openLink"
            v-model:youtube="openYoutube"
            v-model:image="openImage"
            :lang="lang"
            :value="editor?.getAttributes('link').href" 
            :linked="editor?.isActive('link') && editor?.getAttributes('link').href"
            @edit="toogleLink" 
          />
        </div>
        <span 
          v-if="props.options.enableBold || props.options.enableItalic || props.options.enableStrike || props.options.enableCode || props.options.enableUnderline || props.options.enableTextColor || props.options.enableHighlight || props.options.enableLink"
          class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20 pi-tiptap-divide-3"></span>

        <!-- Group 4 -->
        <div v-if="props.options.enableSuperscript">
          <!-- Superscript -->
          <button 
            class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
            @click.prevent="editor?.commands.toggleSuperscript" 
            :title="trans[lang].superscript"
            :aria-label="trans[lang].superscript"
          >
            <SuperscriptIcon class="w-5 h-5" />
          </button>
        </div>
        <div v-if="props.options.enableSubscript">
          <!-- Subscript -->
          <button 
            class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
            @click.prevent="editor?.commands.toggleSubscript" 
            :title="trans[lang].subscript"
            :aria-label="trans[lang].subscript"
          >
            <SubscriptIcon class="w-5 h-5" />
          </button>
        </div>
        <span 
          v-if="props.options.enableSuperscript || props.options.enableSubscript"
          class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20 pi-tiptap-divide-4"></span>

        <!-- Group 5 -->
        <template v-if="props.options.enableTextAlign">
          <!-- Align left -->
          <button 
            class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
            @click.prevent="editor?.commands.toggleTextAlign('left')" 
            :title="trans[lang].alignLeft"
            :aria-label="trans[lang].alignLeft"
          >
            <AlignLeftIcon class="w-5 h-5" />
          </button>
          <!-- Align center -->
          <button 
            class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
            @click.prevent="editor?.commands.toggleTextAlign('center')" 
            :title="trans[lang].alignCenter"
            :aria-label="trans[lang].alignCenter"
          >
            <AlignCenterIcon class="w-5 h-5 " />
          </button>
          <!-- Align right -->
          <button 
            class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
            @click.prevent="editor?.commands.toggleTextAlign('right')" 
            :title="trans[lang].alignRight"
            :aria-label="trans[lang].alignRight"
          >
            <AlignRightIcon class="w-5 h-5" />
          </button>
          <!-- Align justify -->
          <button 
            class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
            @click.prevent="editor?.commands.toggleTextAlign('justify')" 
            :title="trans[lang].alignJustify"
            :aria-label="trans[lang].alignJustify"
          >
            <AlignJustifyIcon class="w-5 h-5" />
          </button>
          <span class="w-px h-6 bg-neutral-200 dark:bg-neutral-50/20 pi-tiptap-divide-5"></span>
        </template>

        <!-- Group 6 -->
        <div v-if="props.options.enableYoutube">
          <PopoverYoutube
            v-model:heading="openHeading" 
            v-model:listing="openListing"
            v-model:textcolor="openTextColor"
            v-model:highlight="openHighlight"
            v-model:link="openLink"
            v-model:image="openImage"
            v-model:youtube="openYoutube"
            v-model:video="video" 
            :lang="lang"
            @edit="setVideo" 
          />
        </div>
        <div v-if="props.options.enableImage">
          <PopoverImage 
            v-model:heading="openHeading" 
            v-model:listing="openListing"
            v-model:textcolor="openTextColor"
            v-model:highlight="openHighlight"
            v-model:link="openLink"
            v-model:youtube="openYoutube"
            v-model:image="openImage"
            v-model:img="img" 
            :lang="lang"
            @edit="setImage" 
          />
        </div>
      </div>

      <div v-if="props.options.enableDarkMode">
        <!-- dark/light mode -->
        <button 
          class="rounded-md hover:bg-neutral-200 dark:hover:bg-neutral-100/20 p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 min-w-8 justify-center text-neutral-700 dark:text-neutral-50"
          @click.prevent="toggleDarkMode" 
          :title="trans[lang].toggleDarkMode"
          :aria-label="trans[lang].toggleDarkMode"
        >
          <LightIcon v-if="mode === 'light'" class="w-5 h-5" />
          <DarkIcon v-else class="w-5 h-5" />
        </button>
      </div>
    </div>

    <!-- Editor -->
    <EditorContent :editor="editor" class="h-full w-full overflow-auto mb-9 lg:mb-0 pi-tiptap-editor-content" />

    <div ref="mobileToolbar" class="lg:hidden">
      <MobileHeadling v-if="props.options.enableHeading"
        :lang="lang"
        :open="openHeading"
        :value="headingValue" 
        @edit="toogleHeading"
      />
      <MobileList v-if="props.options.enableList"
        :lang="lang"
        :open="openListing"
        :value="listingValue" 
        @edit="toogleListing"
      />
      <MobileTextColor v-if="props.options.enableTextColor"
        :lang="lang"
        :open="openTextColor"
        :value="editor?.getAttributes('textStyle').color" 
        @edit="editor?.commands.setColor($event)" 
      />
      <MobileHighlight v-if="props.options.enableHighlight"
        :lang="lang"
        :open="openHighlight"
        :value="editor?.getAttributes('textStyle').background" 
        @edit="toogleHighlight"
      />
      <MobileLink v-if="props.options.enableLink"
        :lang="lang"
        :open="openLink"
        :value="editor?.getAttributes('link').href" 
        :linked="editor?.isActive('link') && editor?.getAttributes('link').href"
        @edit="toogleLink"
      />
      <MobileYoutube v-if="props.options.enableYoutube"
        :lang="lang"
        :open="openYoutube"
        v-model:video="video"
        @edit="setVideo"
        @close="openYoutube = false"
      />
      <MobileImage v-if="props.options.enableImage"
        :lang="lang"
        :open="openImage"
        v-model:img="img"
        @edit="setImage"
        @close="openImage = false"
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
