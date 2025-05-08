<script setup lang="ts">
import NoneIcon from '../icons/NoneIcon.vue'

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
  colors: {
    type: Array as () => string[],
    default: () => [
      // basic colors (
      //  NONE
      //  black, white, red, green, blue, yellow, magenta, cyan, 
      //  orange, purple, pink, brown
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
    ],
  },
})

const handleClick = (value: unknown) => {
  emit('edit', value)
}
</script>
<template>
  <div
    v-show="props.open"
    class="absolute left-0 z-20 bottom-10 w-full p-2 bg-white dark:bg-neutral-900/60 backdrop-blur backdrop-filter transition overflow-x-auto"
  >
    <div class="flex items-center gap-2 min-w-max">
      <div v-for="color in props.colors" :key="color"
        class="flex items-center justify-center w-10 h-10 border-3 rounded-full cursor-pointer outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 hover:opacity-80 shadow"
        :class="{
          'p-1.5': props.value === color,
        }"
        :style="{
          borderColor: props.value && props.value === color ? color : 'rgba(255, 255, 255, 0.25)',
        }"
        :title="color ? color : 'None'"
        :aria-label="color ? color : 'None'"
      >
        <NoneIcon v-if="!color" 
          class="w-full h-full rounded-full outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 shadow text-neutral-700 dark:text-white/50"
          :style="{ 
            backgroundColor: 'rgba(255, 255, 255, 0.25)' 
          }"
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
  </div>
</template>
