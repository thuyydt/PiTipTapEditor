<script setup lang="ts">
import NoneIcon from '../icons/NoneIcon.vue'

const emit = defineEmits(['edit'])
const props = defineProps({
  open: { type: Boolean, default: false, },
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

const handleClick = (value: unknown) => {
  emit('edit', value)
}
</script>
<template>
  <div
    v-show="props.open"
    class="absolute left-0 z-10 bottom-10 w-full p-2 bg-white dark:bg-neutral-900/80 backdrop-blur backdrop-filter transition overflow-x-auto"
  >
    <div class="flex items-center gap-2 min-w-max">
      <template v-for="color in props.colors" :key="color">
        <div
          class="flex items-end justify-center rounded-full cursor-pointer outline-none focus:outline-none ring-0 transition-all duration-300 ease-in-out delay-75 hover:opacity-80"
          :class="{
            'border-2': color,
            'w-11 h-11': color && props.value === color,
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
        <span v-if="!color" class="h-8 w-px dark:bg-neutral-50/20"></span>
      </template>
    </div>
  </div>
</template>
