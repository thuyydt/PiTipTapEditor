<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import HIcon from '../icons/HIcon.vue'
import H1Icon from '../icons/H1Icon.vue'
import H2Icon from '../icons/H2Icon.vue'
import H3Icon from '../icons/H3Icon.vue'
import H4Icon from '../icons/H4Icon.vue'
import DropdownIcon from '../icons/DropdownIcon.vue'

const emit = defineEmits(['mobile', 'edit'])
const props = defineProps({
  value: {
    type: String,
    default: '',
  },
})

const heading = defineModel('heading', { type: Boolean, default: false })
const listing = defineModel('listing', { type: Boolean, default: false })
const textcolor = defineModel('textcolor', { type: Boolean, default: false })
const highlight = defineModel('highlight', { type: Boolean, default: false })
const link = defineModel('link', { type: Boolean, default: false })
const image = defineModel('image', { type: Boolean, default: false })

const handleClick = (value: unknown, close: () => void) => {
  emit('edit', value)

  close()
}

const mobileHandle = () => {
  heading.value = !heading.value
  
  if (heading.value) {
    listing.value = false
    textcolor.value = false
    highlight.value = false
    link.value = false
    image.value = false
  }
}
</script>
<template>
  <Popover v-slot="{ open, close }" class="relative">
    <PopoverButton 
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 transition-all duration-300 delay-75 ease-in-out text-neutral-700 dark:text-neutral-50 pi-toolbar-heading"
      :class="{
        'bg-neutral-200 dark:bg-neutral-100/20': open || ['h1', 'h2', 'h3', 'h4'].includes(props.value),
        'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !open && !['h1', 'h2', 'h3', 'h4'].includes(props.value),
      }"
      title="Heading"
      aria-label="Heading"
      @click="mobileHandle"
    >
      <HIcon v-if="!['h1','h2','h3','h4'].includes(props.value)" class="h-5 w-5" />
      <H1Icon v-else-if="props.value === 'h1'" class="h-5 w-5" />
      <H2Icon v-else-if="props.value === 'h2'" class="h-5 w-5" />
      <H3Icon v-else-if="props.value === 'h3'" class="h-5 w-5" />
      <H4Icon v-else-if="props.value === 'h4'" class="h-5 w-5" />
      <DropdownIcon 
        :class="`h-3 w-3 transition-all duration-300 delay-75 ease-in-out ${open ? 'rotate-180' : ''}`" 
      />
    </PopoverButton>

    <transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="translate-y-1 opacity-0"
      enter-to-class="translate-y-0 opacity-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="translate-y-0 opacity-100"
      leave-to-class="translate-y-1 opacity-0"
    >
      <PopoverPanel
        class="absolute left-0 z-10 bottom-9 lg:bottom-auto lg:mt-1.5 w-fit p-2 border border-neutral-100 dark:border-neutral-100/20 bg-white dark:bg-neutral-900/60 backdrop-blur backdrop-filter rounded-2xl shadow-lg transition hidden lg:block"
      >
        <div class="flex flex-col gap-1">
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h1',
              'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h1',
            }"
            @click.prevent="handleClick('h1', close)"
            title="Heading 1"
            aria-label="Heading 1"
          >
            <H1Icon class="h-4 w-4" />
            <span>Heading 1</span>
          </button>
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h2',
              'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h2',
            }"
            @click.prevent="handleClick('h2', close)"
            title="Heading 2"
            aria-label="Heading 2"
          >
            <H2Icon class="h-4 w-4" />
            <span>Heading 2</span>
          </button>
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h3',
              'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h3',
            }"
            @click.prevent="handleClick('h3', close)"
            title="Heading 3"
            aria-label="Heading 3"
          >
            <H3Icon class="h-4 w-4" />
            <span>Heading 3</span>
          </button>
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h4',
              'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h4',
            }"
            @click.prevent="handleClick('h4', close)"
            title="Heading 4"
            aria-label="Heading 4"
          >
            <H4Icon class="h-4 w-4" />
            <span>Heading 4</span>
          </button>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>
