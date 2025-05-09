<script setup lang="ts">
import LinkTargetIcon from '../icons/LinkTargetIcon.vue'

import ja from '../lang/ja.json'
import en from '../lang/en.json'
import vi from '../lang/vi.json'
// register i18n
const trans: Record<string, Record<string, string>> = {
  ja,
  en,
  vi,
}

const emit = defineEmits(['edit', 'close'])
const props = defineProps({
  lang: { type: String, default: 'en' },
  open: {
    type: Boolean,
    default: false,
  },
})

const video = defineModel('video', { type: String, default: '', })

const handleClick = () => {
  emit('edit', video.value)

  close()
}

const close = () => {
  emit('close')
}
</script>
<template>
  <div v-show="props.open"
    class="absolute left-0 transform z-10 bottom-10 w-full p-2 bg-white dark:bg-neutral-900/80 backdrop-blur backdrop-filter transition border-t border-neutral-100 dark:border-neutral-100/20"
  >
    <div class="flex flex-col w-full gap-2">
      <div class="flex items-center gap-2 w-full">
        <div class="grow">
          <div class="flex items-center gap-1 relative">
            <span class="absolute top-1/2 left-px transform -translate-y-1/2 text-neutral-400 dark:text-neutral-50 bg-neutral-50/20 rounded-l-sm px-1 h-full flex items-center">
              {{ trans[props.lang].url }}
            </span>
            <input
              type="text"
              class="w-full bg-neutral-200 dark:bg-white/20 text-neutral-700 dark:text-neutral-50 outline-none focus:outline-none ring-0 pl-12 pr-8 py-0.5 rounded"
              placeholder="https://www.youtube.com/watch?v=XXXXXX"
              v-model="video"
              @keyup.enter="handleClick"
            />
            <div class="absolute top-1/2 right-1 transform -translate-y-1/2 flex p-1 items-center group rounded-lg hover:bg-neutral-200 dark:hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 leading-none h-fit cursor-pointer">
              <a 
                :href="video" 
                target="_blank"
                :title="trans[props.lang].preview"
              >
                <LinkTargetIcon class="w-5 h-5 text-neutral-400 dark:text-neutral-50/40"/>
              </a>
            </div>
          </div>
        </div>
      </div>
      <div class="flex items-center justify-center gap-2">
        <div>
          <button 
            type="button" 
            class="group p-1 rounded-lg hover:opacity-80 bg-blue-500 transition-all duration-200 ease-in-out delay-75 text-neutral-50 cursor-pointer px-2 min-w-24" 
            @click.prevent="handleClick"
          >
            {{ trans[props.lang].ok }}
          </button>
        </div>
        <div>
          <button 
            type="button" 
            class="group p-1 rounded-lg hover:opacity-80 bg-neutral-200 dark:bg-neutral-50/20 transition-all duration-200 ease-in-out delay-75 text-neutral-700 dark:text-neutral-50 cursor-pointer px-2 min-w-24" 
            @click.prevent="close"
          >
            {{ trans[props.lang].cancel }}
          </button>
        </div>
      </div>
      
    </div>
  </div>
</template>
