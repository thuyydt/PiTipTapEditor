<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import { ListBulletIcon, ChevronDownIcon, NumberedListIcon } from '@heroicons/vue/24/outline'

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
        'bg-neutral-100/20': open || ['bullet', 'number'].includes(props.value),
        'hover:bg-neutral-100/20': !open && !['bullet', 'number'].includes(props.value),
      }"
    >
      <NumberedListIcon v-if="props.value === 'number'" class="h-5 w-5 text-neutral-50" />
      <ListBulletIcon v-else class="h-5 w-5 text-neutral-50" />
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
        class="absolute left-0 z-10 mt-2 w-fit p-2 bg-neutral-900/60 backdrop-blur backdrop-filter rounded-2xl shadow-lg transition"
      >
        <div class="flex flex-col gap-1">
          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'hover:bg-neutral-100/20 text-white/50': props.value !== 'bullet',
              'bg-neutral-100/20 text-white': props.value === 'bullet',
            }"
            @click.prevent="handleClick('bullet', close)"
          >
            <ListBulletIcon class="h-5 w-5" />
            <span>Bullet List</span>
          </button>

          <button 
            class="flex items-center gap-4 min-w-max cursor-pointer rounded-md p-1 outline-none focus:outline-none ring-0" 
            :class="{
              'hover:bg-neutral-100/20 text-white/50': props.value !== 'number',
              'bg-neutral-100/20 text-white': props.value === 'number',
            }"
            @click.prevent="handleClick('number', close)"
          >
            <NumberedListIcon class="h-5 w-5" />
            <span>Numbered List</span>
          </button>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>