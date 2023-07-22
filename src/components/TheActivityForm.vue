<script setup>
import { ref, inject, nextTick } from 'vue'
import BaseButton from '@/components/BaseButton.vue'
import { PlusIcon } from '@heroicons/vue/24/outline'
import { createActivityKey } from '@/keys'
import { id } from '@/functions'

const name = ref('')

const createActivity = inject(createActivityKey)

async function submit() {
  createActivity({
    id: id(),
    name: name.value,
    secondsToComplete: 0
  })

  name.value = ''

  await nextTick(() => {
    window.scrollTo(0, document.body.scrollHeight)
  })
}
</script>
<template>
  <form @submit.prevent="submit" class="sticky bottom-[56px] flex gap-2 border-t bg-white p-4">
    <input
      type="text"
      v-model="name"
      class="w-full rounded border px-4 text-xl"
      placeholder="Activity name"
    />
    <BaseButton :disabled="name.trim() === ''" placeholder="" options="">
      <PlusIcon class="h-8"
    /></BaseButton>
  </form>
</template>
