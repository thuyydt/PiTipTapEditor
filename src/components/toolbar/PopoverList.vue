<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'

import BulletListIcon from '../icons/BulletListIcon.vue'
import OrderedListIcon from '../icons/OrderedListIcon.vue'
import TaskListIcon from '../icons/TaskListIcon.vue'
import DropdownIcon from '../icons/DropdownIcon.vue'

const emit = defineEmits(['edit'])

const props = defineProps({
  value: { type: String, default: '' },
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
  listing.value = !listing.value
  if (listing.value) {
    heading.value = false
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
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 transition-all duration-300 delay-75 ease-in-out text-neutral-700 dark:text-neutral-50 pi-toolbar-list"
      :class="{
        'bg-neutral-200 dark:bg-neutral-100/20': open || ['bullet', 'number'].includes(props.value),
        'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !open && !['bullet', 'number'].includes(props.value),
      }"
      title="List"
      aria-label="List"
      @click="mobileHandle"
    >
      <OrderedListIcon v-if="props.value === 'number'" class="h-5 w-5" />
      <BulletListIcon v-else class="h-5 w-5 " />
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
        class="absolute left-0 z-10 bottom-9 lg:bottom-auto lg:mt-2 w-fit p-2 border border-neutral-100 dark:border-neutral-100/20 bg-white dark:bg-neutral-900/60 backdrop-blur backdrop-filter rounded-2xl shadow-lg transition hidden lg:block"
      >
        <div class="flex flex-col gap-1">
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'bullet',
              'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'bullet',
            }"
            @click.prevent="handleClick('bullet', close)"
          >
            <BulletListIcon class="h-5 w-5" />
            <span>Bullet List</span>
          </button>

          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'number',
              'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'number',
            }"
            @click.prevent="handleClick('number', close)"
          >
            <OrderedListIcon class="h-5 w-5" />
            <span>Numbered List</span>
          </button>

          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'task',
              'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'task',
            }"
            @click.prevent="handleClick('task', close)"
          >
            <TaskListIcon class="h-5 w-5" />
            <span>Task List</span>
          </button>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>