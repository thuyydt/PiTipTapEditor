<script setup lang="ts">
import BulletListIcon from '../icons/BulletListIcon.vue'
import OrderedListIcon from '../icons/OrderedListIcon.vue'
import TaskListIcon from '../icons/TaskListIcon.vue'

// import ja from '../lang/ja.json'
// import en from '../lang/en.json'
// import vi from '../lang/vi.json'
// // register i18n
// const trans: Record<string, Record<string, string>> = {
//   ja,
//   en,
//   vi,
// }

const emit = defineEmits(['edit'])
const props = defineProps({
  lang: { type: String, default: 'en' },
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
    class="absolute left-0 z-10 bottom-10 w-full border-t border-neutral-100 dark:border-neutral-100/20 p-2 bg-white dark:bg-neutral-900/80 backdrop-blur backdrop-filter transition overflow-x-auto"
  >
    <div class="flex items-center gap-4 min-w-full">
      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'bullet',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'bullet',
        }"
        @click.prevent="handleClick('bullet')"
      >
        <BulletListIcon class="h-6 w-6" />
      </button>

      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'number',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'number',
        }"
        @click.prevent="handleClick('number')"
      >
        <OrderedListIcon class="h-6 w-6" />
      </button>

      <button 
        class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
        :class="{
          'text-neutral-400 hover:bg-neutral-200 dark:hover:bg-neutral-100/20 dark:text-white/50': props.value !== 'task',
          'bg-neutral-200 text-neutral-700 dark:bg-neutral-100/20 dark:text-white': props.value === 'task',
        }"
        @click.prevent="handleClick('task')"
      >
        <TaskListIcon class="h-6 w-6" />
      </button>
    </div>
  </div>
</template>