<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import { PaintBrushIcon, NoSymbolIcon } from '@heroicons/vue/24/outline'

const emit = defineEmits(['edit'])
const props = defineProps({
  value: {
    type: String,
    default: '',
  },
  colors: {
    type: Array as () => string[],
    default: () => [
      // basic colors (
      //  NONE
      //  black, white, red, green, blue, yellow, magenta, cyan, 
      //  orange, purple, pink, brown, gray
      // )
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
      '#A52A2A',
      '#808080',
    ],
  },
})

const handleClick = (value: unknown) => {
  emit('edit', value)
}
</script>
<template>
  <Popover v-slot="{ open }" class="relative">
    <PopoverButton 
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 transition-all duration-300 delay-75 ease-in-out"
      :class="{
        'bg-neutral-100/20': open || props.value,
        'hover:bg-neutral-100/20': !open && !props.value,
      }"
    >
      <PaintBrushIcon class="w-5 h-5 text-neutral-50" />
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
        class="absolute left-1/2 -translate-x-1/2 transform z-10 mt-2 w-fit p-2 bg-neutral-900/60 backdrop-filter backdrop-blur rounded-2xl shadow-lg transition"
      >
        <div class="flex flex-wrap items-center gap-2 min-w-sm">
          <div v-for="color in props.colors" :key="color"
            class="flex items-center justify-center w-10 h-10 border-2 rounded-full cursor-pointer outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 hover:opacity-80 shadow"
            :class="{
              'p-0.5': props.value === color,
            }"
            :style="{
              borderColor: props.value && props.value === color ? color : 'rgba(255, 255, 255, 0.25)',
            }"
            :title="color ? color : 'None'"
            :aria-label="color ? color : 'None'"
          >
            <NoSymbolIcon v-if="!color" 
              class="w-full h-full rounded-full outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 shadow text-white/50"
              :style="{ 
                backgroundColor: 'rgba(255, 255, 255, 0.25)' 
              }"
              @click.prevent="handleClick('')"
            />
            <button v-else
              type="button"
              class="w-full h-full rounded-full outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 shadow"
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
