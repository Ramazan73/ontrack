<script setup>
import { ref, nextTick } from 'vue'
import BaseButton from '@/components/BaseButton.vue'
import BaseIcon from './BaseIcon.vue'
import { createActivity } from '@/activities'
import { id } from '@/functions'
import { ICON_PLUS } from '../icons'

const name = ref('')

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
      <BaseIcon :name="ICON_PLUS"
    /></BaseButton>
  </form>
</template>
