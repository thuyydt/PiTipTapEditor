<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import { ChevronDownIcon } from '@heroicons/vue/24/outline'

const emit = defineEmits(['edit'])
const props = defineProps({
  value: {
    type: String,
    default: '',
  },
})

const handleClick = (value: unknown, close: () => void) => {
  emit('edit', value)

  close()
}
</script>
<template>
  <Popover v-slot="{ open, close }" class="relative">
    <PopoverButton 
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 transition-all duration-300 delay-75 ease-in-out"
      :class="{
        'bg-neutral-100/20': open || ['h1', 'h2', 'h3', 'h4'].includes(props.value),
        'hover:bg-neutral-100/20': !open && !['h1', 'h2', 'h3', 'h4'].includes(props.value),
      }"
    >
      <span v-if="!['h1','h2','h3','h4'].includes(props.value)" class="text-neutral-50 w-8 text-end">H</span>
      <span v-else-if="props.value === 'h1'" class="text-neutral-50 text-xs w-8 text-end">
        H<sub>1</sub>
      </span>
      <span v-else-if="props.value === 'h2'" class="text-neutral-50 text-xs w-8 text-end">
        H<sub>2</sub>
      </span>
      <span v-else-if="props.value === 'h3'" class="text-neutral-50 text-xs w-8 text-end">
        H<sub>3</sub>
      </span>
      <span v-else-if="props.value === 'h4'" class="text-neutral-50 text-xs w-8 text-end">
        H<sub>4</sub>
      </span>
      <ChevronDownIcon 
        class="h-3 w-3 text-neutral-50 transition-all duration-300 delay-75 ease-in-out" 
        :class="{
          'rotate-180': open,
        }"
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
        class="absolute left-0 z-10 mt-1.5 w-fit p-2 bg-neutral-900/60 backdrop-blur backdrop-filter rounded-2xl shadow-lg transition"
      >
        <div class="flex flex-col gap-1">
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'hover:bg-neutral-100/20 text-white/50': props.value !== 'h1',
              'bg-neutral-100/20 text-white': props.value === 'h1',
            }"
            @click.prevent="handleClick('h1', close)"
          >
            <span class="flex items-center gap-1 text-xs">
              H
              <sub>1</sub>
            </span>
            <span>Heading 1</span>
          </button>
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'hover:bg-neutral-100/20 text-white/50': props.value !== 'h2',
              'bg-neutral-100/20 text-white': props.value === 'h2',
            }"
            @click.prevent="handleClick('h2', close)"
          >
            <span class="flex items-center gap-1 text-xs">
              H<sub>2</sub>
            </span>
            <span>Heading 2</span>
          </button>
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'hover:bg-neutral-100/20 text-white/50': props.value !== 'h3',
              'bg-neutral-100/20 text-white': props.value === 'h3',
            }"
            @click.prevent="handleClick('h3', close)"
          >
            <span class="flex items-center gap-1 text-xs">
              H<sub>3</sub>
            </span>
            <span>Heading 3</span>
          </button>
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'hover:bg-neutral-100/20 text-white/50': props.value !== 'h4',
              'bg-neutral-100/20 text-white': props.value === 'h4',
            }"
            @click.prevent="handleClick('h4', close)"
          >
            <span class="flex items-center gap-1 text-xs">
              H<sub>4</sub>
            </span>
            <span>Heading 4</span>
          </button>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>
