<script setup>
import { nextTick, watchPostEffect } from 'vue'
import { PAGE_TIMELINE } from '@/constants'
import TimelineItem from '@/components/TimelineItem.vue'
import {
  timelineItems,
  timelineItemRefs,
  scrollToHour,
  scrollToCurrentHour
} from '@/timeline-items'

import { currentPage } from '@/router'

watchPostEffect(async () => {
  if (currentPage.value === PAGE_TIMELINE) {
    await nextTick()

    scrollToCurrentHour(true)
  }
})
</script>

<template>
  <div class="mt-7">
    <ul>
      <TimelineItem
        v-for="timelineItem in timelineItems"
        :key="timelineItem.hour"
        :timeline-item="timelineItem"
        @scroll-to-hour="scrollToHour(timelineItem.hour)"
        ref="timelineItemRefs"
      />
    </ul>
  </div>
</template>
