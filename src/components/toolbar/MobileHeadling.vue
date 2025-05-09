<script setup lang="ts">
import H1Icon from '../icons/H1Icon.vue'
import H2Icon from '../icons/H2Icon.vue'
import H3Icon from '../icons/H3Icon.vue'
import H4Icon from '../icons/H4Icon.vue'

const emit = defineEmits(['edit'])
const props = defineProps({
  open: {
    type: Boolean,
    default: false,
  },
  value: {
    type: String,
    default: '',
  },
})

const handleClick = (value: unknown) => {
  emit('edit', value)
}
</script>
<template>
  <div 
    v-show="props.open"
    class="absolute left-0 z-10 bottom-10 w-full border-t border-neutral-100 dark:border-neutral-100/20 p-2 bg-white/80 dark:bg-neutral-900/60 backdrop-blur backdrop-filter transition overflow-x-auto"
  >
    <div class="flex items-center min-w-full gap-4">
      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h1',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h1',
        }"
        @click.prevent="handleClick('h1')"
        title="Heading 1"
        aria-label="Heading 1"
      >
        <H1Icon class="h-6 w-6" />
      </button>
      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h2',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h2',
        }"
        @click.prevent="handleClick('h2')"
        title="Heading 2"
        aria-label="Heading 2"
      >
        <H2Icon class="h-6 w-6" />
      </button>
      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h3',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h3',
        }"
        @click.prevent="handleClick('h3')"
        title="Heading 3"
        aria-label="Heading 3"
      >
        <H3Icon class="h-6 w-6" />
      </button>
      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h4',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h4',
        }"
        @click.prevent="handleClick('h4')"
        title="Heading 4"
        aria-label="Heading 4"
      >
        <H4Icon class="h-6 w-6" />
      </button>
    </div>
  </div>
</template>
