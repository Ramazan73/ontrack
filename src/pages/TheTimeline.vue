<script setup>
import { nextTick, ref, watchPostEffect } from 'vue'
import { MIDNIGHT_HOUR, PAGE_TIMELINE } from '@/constants'

import TimelineItem from '@/components/TimelineItem.vue'
import { validateTimelineItems } from '@/validators'
import { currentPage } from '@/router'

defineProps({
  timelineItems: {
    required: true,
    type: Array,
    validator: validateTimelineItems
  }
})

defineExpose({ scrollToHour })

const timelineItemRefs = ref([])

watchPostEffect(async () => {
  if (currentPage.value === PAGE_TIMELINE) {
    await nextTick()

    scrollToHour(null, true)
  }
})

function scrollToHour(hour = null, isSmooth = true) {
  hour ??= new Date().getHours()

  const el = hour === MIDNIGHT_HOUR ? document.body : timelineItemRefs.value[hour - 1].$el

  el.scrollIntoView({ behavior: isSmooth ? 'smooth' : 'instant' })
}
</script>

<template>
  <div class="mt-7">
    <ul>
      <TimelineItem
        v-for="timelineItem in timelineItems"
        :key="timelineItem.hour"
        :timeline-item="timelineItem"
        @scroll-to-hour="scrollToHour"
        ref="timelineItemRefs"
      />
    </ul>
  </div>
</template>
