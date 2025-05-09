<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import LinkTargetIcon from '../icons/LinkTargetIcon.vue'
import ImageIcon from '../icons/ImageIcon.vue'

const emit = defineEmits(['edit'])

const img = defineModel('img', { type: String, default: '' })
const heading = defineModel('heading', { type: Boolean, default: false })
const listing = defineModel('listing', { type: Boolean, default: false })
const textcolor = defineModel('textcolor', { type: Boolean, default: false })
const highlight = defineModel('highlight', { type: Boolean, default: false })
const link = defineModel('link', { type: Boolean, default: false })
const image = defineModel('image', { type: Boolean, default: false })

const handleClick = (close: () => void) => {
  emit('edit', img.value)
  close()
}

const mobileHandle = () => {
  image.value = !image.value
  if (image.value) {
    heading.value = false
    listing.value = false
    textcolor.value = false
    highlight.value = false
    link.value = false
  }
}
</script>
<template>
  <Popover v-slot="{ close }" class="relative">
    <PopoverButton 
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 text-neutral-700 dark:text-neutral-50 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 transition-all duration-300 delay-75 ease-in-out pi-toolbar-image"
      title="Add image"
      aria-label="Add image"
      @click="mobileHandle"
    >
      <ImageIcon class="w-5 h-5" />
      <span class="text-xs">Add</span>
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
        class="absolute right-0 transform z-10 bottom-9 lg:bottom-auto lg:mt-2 w-fit p-2 bg-white dark:bg-neutral-900/60 backdrop-blur backdrop-filter transition rounded-2xl shadow-lg border border-neutral-100 dark:border-neutral-100/20 hidden lg:block"
      >
        <div class="flex flex-col gap-2 p-2">
          <div class="flex flex-col">
            <span class="text-neutral-700 dark:text-neutral-50">
              URL
            </span>
            <div class="w-64 lg:w-80 flex gap-2">
              <div class="grow">
                <input
                  type="text"
                  class="w-full bg-neutral-200 dark:bg-white/20 text-neutral-700 dark:text-neutral-50 outline-none focus:outline-none ring-0 px-2 py-0.5 rounded"
                  placeholder="https://example.com/img.png"
                  v-model="img"
                  @keyup.enter="handleClick(close)"
                />
              </div>
              <div class="flex-none w-fit">
                <div class="flex p-1 items-center group rounded-lg hover:bg-neutral-200 dark:hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 leading-none h-fit cursor-pointer">
                  <a 
                    :href="img" 
                    target="_blank"
                    title="Preview image"
                  >
                    <LinkTargetIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50"/>
                  </a>
                </div>
              </div>
            </div>
          </div>
          <div class="flex items-center gap-4">
            <div class="flex items-center gap-0.5">
              <span class="text-neutral-700 dark:text-neutral-50">Width</span>
              <div>
                <input
                  type="number"
                  class="w-20 bg-neutral-200 dark:bg-white/20 text-neutral-700 dark:text-neutral-50 outline-none focus:outline-none ring-0 px-2 py-0.5 rounded"
                  placeholder="Width"
                />
              </div>
              <span class="text-neutral-700 dark:text-neutral-50">px</span>
            </div>
            <div class="flex items-center gap-0.5">
              <span class="text-neutral-700 dark:text-neutral-50">Height</span>
              <div>
                <input
                  type="number"
                  class="w-20 bg-neutral-200 dark:bg-white/20 text-neutral-700 dark:text-neutral-50 outline-none focus:outline-none ring-0 px-2 py-0.5 rounded"
                  placeholder="Height"
                />
              </div>
              <span class="text-neutral-700 dark:text-neutral-50">px</span>
            </div>
          </div>
          <div class="text-xs text-neutral-400 dark:text-neutral-50/50">
            Leave blank the Width and the Height to use the original size.
          </div>
          <div class="flex items-center justify-center gap-2 mt-4">
            <div>
              <button 
                type="button" 
                class="group p-1 rounded-lg hover:opacity-80 bg-blue-500 transition-all duration-200 ease-in-out delay-75 text-neutral-50 cursor-pointer px-4 min-w-24" 
                @click.prevent="handleClick(close)"
              >
                OK
              </button>
            </div>
            <div>
              <button 
                type="button" 
                class="group p-1 rounded-lg hover:opacity-80 bg-neutral-200 dark:bg-neutral-50/20 transition-all duration-200 ease-in-out delay-75 text-neutral-700 dark:text-neutral-50 cursor-pointer px-4 min-w-24" 
                @click.prevent="close"
              >
                Cancel
              </button>
            </div>
          </div>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>
