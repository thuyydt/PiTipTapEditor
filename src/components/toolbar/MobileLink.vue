<script setup lang="ts">
import LinkTargetIcon from '../icons/LinkTargetIcon.vue'
import TrashIcon from '../icons/TrashIcon.vue'
import EnterIcon from '../icons/EnterIcon.vue'

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
  linked: {
    type: Boolean,
    required: true,
    default: false,
  },
})

const handleClick = (value: unknown) => {
  emit('edit', value)
}
</script>
<template>
  <div v-show="props.open"
    class="absolute left-0 z-10 bottom-10 w-full px-2 py-1 border-t border-neutral-100 dark:border-neutral-100/20 bg-white/80 dark:bg-neutral-900/60 backdrop-blur backdrop-filter transition"
  >
    <div class="flex flex-col divide-y divide-neutral-200 dark:divide-neutral-200/20">
      <div class="w-full relative flex items-center pb-1">
        <input
          type="text"
          class="w-full bg-transparent text-neutral-700 dark:text-neutral-50 outline-none focus:outline-none ring-0"
          placeholder="https://example.com"
          :value="props.value"
          @keyup.enter="handleClick(($event.target as HTMLInputElement)?.value || '')"
        />
        <div class="absolute right-1 flex items-center">
          <EnterIcon 
            class="w-4 h-4 text-neutral-300 dark:text-neutral-50/20 cursor-pointer hover:opacity-80 transition-all duration-200 ease-in-out delay-75" 
          />
        </div>
      </div>

      <div class="flex items-center divide-x divide-neutral-200 dark:divide-neutral-200/20 pt-1 w-full">
        <div class="flex items-center justify-between gap-2 w-full">
          <div class="flex items-center gap-2">
            <div class="flex items-center">
              <button 
                type="button" 
                class="group p-1 rounded-lg bg-blue-500 hover:opacity-80 text-neutral-50 transition-all duration-200 ease-in-out delay-75 cursor-pointer min-w-20" 
                @click.prevent="handleClick('link')"
              >
                OK
              </button>
            </div>
            <div class="flex p-1 items-center group rounded-lg hover:bg-neutral-200 dark:hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 leading-none h-fit cursor-pointer">
              <a 
                :href="props.value" 
                target="_blank"
                title="Preview link"
                :aria-label="`Preview link ${props.value}`"
              >
                <LinkTargetIcon class="w-5 h-5 text-neutral-700 dark:text-neutral-50"/>
              </a>
            </div>
          </div>
          <div class="flex items-center">
            <button 
              type="button" 
              class="group p-1 rounded-lg hover:bg-neutral-200 hover:text-red-400 dark:hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 cursor-pointer text-neutral-700 dark:text-neutral-50" 
              @click.prevent="handleClick('unlink')"
              title="Unlink"
              :aria-label="`Unlink ${props.value}`"
            >
              <TrashIcon class="w-5 h-5"/>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>