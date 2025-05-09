<script setup lang="ts">
import H1Icon from '../icons/H1Icon.vue'
import H2Icon from '../icons/H2Icon.vue'
import H3Icon from '../icons/H3Icon.vue'
import H4Icon from '../icons/H4Icon.vue'

import ja from '../lang/ja.json'
import en from '../lang/en.json'
import vi from '../lang/vi.json'
// register i18n
const trans: Record<string, Record<string, string>> = {
  ja,
  en,
  vi,
}

const emit = defineEmits(['edit'])
const props = defineProps({
  lang: { type: String, default: 'en' },
  open: { type: Boolean, default: false },
  value: { type: String, default: '', },
})

const handleClick = (value: unknown) => {
  emit('edit', value)
}
</script>
<template>
  <div 
    v-show="props.open"
    class="absolute left-0 z-10 bottom-10 w-full border-t border-neutral-100 dark:border-neutral-100/20 p-2 bg-white dark:bg-neutral-900/80 backdrop-blur backdrop-filter transition overflow-x-auto"
  >
    <div class="flex items-center min-w-full gap-4">
      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'h1',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'h1',
        }"
        @click.prevent="handleClick('h1')"
        :title="trans[props.lang].heading1"
        :aria-label="trans[props.lang].heading1"
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
        :title="trans[props.lang].heading2"
        :aria-label="trans[props.lang].heading2"
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
        :title="trans[props.lang].heading3"
        :aria-label="trans[props.lang].heading3"
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
        :title="trans[props.lang].heading4"
        :aria-label="trans[props.lang].heading4"
      >
        <H4Icon class="h-6 w-6" />
      </button>
    </div>
  </div>
</template>
