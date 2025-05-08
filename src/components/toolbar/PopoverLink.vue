<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import LinkIcon from '../icons/LinkIcon.vue'
import LinkTargetIcon from '../icons/LinkTargetIcon.vue'
import TrashIcon from '../icons/TrashIcon.vue'
import EnterIcon from '../icons/EnterIcon.vue'

const emit = defineEmits(['edit'])

const props = defineProps({
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

const handleClick = (value: unknown, close: () => void) => {
  emit('edit', value)

  close()
}
</script>
<template>
  <Popover v-slot="{ open, close }" class="relative">
    <PopoverButton 
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 transition-all duration-300 delay-75 ease-in-out text-neutral-700 dark:text-neutral-50"
      :class="{
        'hover:bg-neutral-200 dark:hover:bg-neutral-100/20': !open && !props.linked,
        'bg-neutral-200 dark:bg-neutral-100/20': open || props.linked,
      }"
      :title="props.linked ? 'Unlink' : 'Link'"
      :aria-label="props.linked ? 'Unlink' : 'Link'"
    >
      <LinkIcon class="w-5 h-5" />
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
        class="absolute left-1/2 -translate-x-1/2 transform z-10 bottom-9 md:bottom-auto md:mt-2 w-fit p-2 border border-neutral-100 dark:border-neutral-100/20 bg-white dark:bg-neutral-900/60 backdrop-blur backdrop-filter transition rounded-2xl shadow-lg"
      >
        <div class="flex items-center divide-x divide-neutral-200 dark:divide-neutral-200/20 px-2">
          <div class="w-40 md:w-64 relative flex items-center">
            <input
              type="text"
              class="w-full bg-transparent text-neutral-700 dark:text-neutral-50 outline-none focus:outline-none ring-0"
              placeholder="https://example.com"
              :value="props.value"
              @keyup.enter="handleClick($event.target.value, close)"
            />
            <div class="absolute right-1 flex items-center">
              <EnterIcon 
                class="w-4 h-4 text-neutral-300 dark:text-neutral-50/20 cursor-pointer hover:opacity-80 transition-all duration-200 ease-in-out delay-75" 
              />
            </div>
          </div>
          <div class="flex items-center gap-2">
            <div class="ml-2 flex items-center">
              <button 
                type="button" 
                class="group p-1 rounded-lg bg-blue-500 hover:opacity-80 text-neutral-50 transition-all duration-200 ease-in-out delay-75 cursor-pointer min-w-14" 
                @click.prevent="handleClick('link', close)"
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
            <div class="flex items-center">
              <button 
                type="button" 
                class="group p-1 rounded-lg hover:bg-neutral-200 hover:text-red-400 dark:hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 cursor-pointer text-neutral-700 dark:text-neutral-50" 
                @click.prevent="handleClick('unlink', close)"
                title="Unlink"
                :aria-label="`Unlink ${props.value}`"
              >
                <TrashIcon class="w-5 h-5"/>
              </button>
            </div>
          </div>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>