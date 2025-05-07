<script setup lang="ts">
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import { LinkIcon, TrashIcon, ArrowTopRightOnSquareIcon } from '@heroicons/vue/24/outline'

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
      class="rounded-md p-1 hover:opacity-80 cursor-pointer flex items-center gap-1 outline-none focus:outline-none ring-0 transition-all duration-300 delay-75 ease-in-out"
      :class="{
        'hover:bg-neutral-100/20': !open && !props.linked,
        'bg-neutral-100/20': open || props.linked,
      }"
    >
      <LinkIcon class="w-5 h-5 text-neutral-50" />
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
        class="absolute left-1/2 -translate-x-1/2 transform z-10 mt-2 w-fit p-2 bg-neutral-900/60 backdrop-blur backdrop-filter transition rounded-2xl shadow-lg"
      >
        <div class="flex items-center divide-x divide-neutral-200/20 px-2">
          <div class="w-44 md:w-64">
            <input
              type="text"
              class="w-full bg-transparent text-neutral-50 outline-none focus:outline-none ring-0"
              placeholder="https://example.com"
              :value="props.value"
              @keyup.enter="handleClick($event.target.value, close)"
            />
          </div>
          <div class="flex items-center gap-2">
            <div class="ml-2 flex items-center">
              <button 
                type="button" 
                class="group p-1 rounded-lg hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 text-neutral-50 cursor-pointer" 
                @click.prevent="handleClick('link', close)"
              >
                OK
              </button>
            </div>
            <div class="flex p-1 items-center group rounded-lg hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 leading-none h-fit cursor-pointer">
              <a 
                :href="props.value" 
                target="_blank"
                title="Preview link"
              >
                <ArrowTopRightOnSquareIcon class="w-5 h-5 text-neutral-50"/>
              </a>
            </div>
            <div class="flex items-center">
              <button 
                type="button" 
                class="group p-1 rounded-lg hover:bg-neutral-200/30 transition-all duration-200 ease-in-out delay-75 cursor-pointer" 
                @click.prevent="handleClick('unlink', close)"
                title="Unlink"
              >
                <TrashIcon class="w-5 h-5 text-neutral-50"/>
              </button>
            </div>
          </div>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>