<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'

import ColorIcon from '../icons/ColorIcon.vue'
import NoneIcon from '../icons/NoneIcon.vue'

const emit = defineEmits(['edit'])

const props = defineProps({
  value: { type: String, default: '' },
  colors: {
    type: Array as () => string[],
    default: () => [
      '',
      '#000000',
      '#FFFFFF',
      '#FF0000',
      '#00FF00',
      '#0000FF',
      '#FFFF00',
      '#FF00FF',
      '#00FFFF',
      '#FFA500',
      '#800080',
      '#FFC0CB',
    ],
  },
})

const heading = defineModel('heading', { type: Boolean, default: false })
const listing = defineModel('listing', { type: Boolean, default: false })
const textcolor = defineModel('textcolor', { type: Boolean, default: false })
const highlight = defineModel('highlight', { type: Boolean, default: false })
const link = defineModel('link', { type: Boolean, default: false })
const image = defineModel('image', { type: Boolean, default: false })

const handleClick = (value: unknown) => {
  emit('edit', value)
}

const mobileHandle = () => {
  textcolor.value = !textcolor.value
  if (textcolor.value) {
    heading.value = false
    listing.value = false
    highlight.value = false
    link.value = false
    image.value = false
  }
}
</script>
<template>
  <Popover v-slot="{ open }" class="relative">
    <PopoverButton 
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 transition-all duration-300 delay-75 ease-in-out text-neutral-700 dark:text-neutral-50 pi-toolbar-text-color"
      :class="{
        'bg-neutral-200 dark:bg-neutral-100/20': open || props.value,
        'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !open && !props.value,
      }"
      title="Color"
      aria-label="Color"
      @click="mobileHandle"
    >
      <ColorIcon class="w-5 h-5 fill-neutral-700 dark:fill-neutral-50" />
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
        class="absolute left-1/2 -translate-x-1/2 transform z-10 bottom-9 lg:bottom-auto lg:mt-2 w-fit p-2 bg-white dark:bg-neutral-900/60 backdrop-filter backdrop-blur rounded-2xl shadow-lg transition border border-neutral-100 dark:border-neutral-100/20 hidden lg:block"
      >
        <div class="flex flex-wrap items-start gap-2 min-w-sm">
          <div v-for="color in props.colors" :key="color"
            class="flex items-start justify-center rounded-full cursor-pointer outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 hover:opacity-80 shadow"
            :class="{
              'border-2': color,
              'w-10 h-10': color && props.value === color,
              'w-8 h-8': !color || props.value !== color,
            }"
            :style="{
              borderColor: 'rgba(255, 255, 255, 0.25)',
            }"
            :title="color ? color : 'None'"
            :aria-label="color ? color : 'None'"
          >
            <NoneIcon v-if="!color" 
              class="w-full h-full rounded-full outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 shadow text-neutral-700 dark:text-white/50"
              @click.prevent="handleClick('')"
            />
            <button v-else
              type="button"
              class="w-full h-full rounded-full outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 shadow cursor-pointer"
              :style="{ 
                backgroundColor: color as string 
              }"
              @click.prevent="handleClick(color)"
            ></button>
          </div>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>
