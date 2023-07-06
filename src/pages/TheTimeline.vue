<script setup>
import { nextTick, ref, watchPostEffect } from 'vue'
import { MIDNIGHT_HOUR, PAGE_TIMELINE } from '@/constants'

import TimelineItem from '@/components/TimelineItem.vue'
import {
  validateActivities,
  validateSelectOptions,
  validateTimelineItems,
  isTimelineItemValid,
  isActivityValid,
  isPageValid
} from '@/validators'

const props = defineProps({
  timelineItems: {
    required: true,
    type: Array,
    validator: validateTimelineItems
  },
  activitySelectOptions: {
    required: true,
    type: Array,
    validator: validateSelectOptions
  },
  activities: {
    required: true,
    type: Object,
    validator: validateActivities
  },
  currentPage: {
    required: true,
    type: String,
    validator: isPageValid
  }
})

const emit = defineEmits({
  setTimelineItemActivity(timelineItem, activity) {
    return [isTimelineItemValid(timelineItem), isActivityValid(activity)].every(Boolean)
  }
})

defineExpose({ scrollToHour })

const timelineItemRefs = ref([])

watchPostEffect(async () => {
  if (props.currentPage === PAGE_TIMELINE) {
    await nextTick()

    scrollToHour(null, true)
  }
})

function scrollToHour(hour = null, isSmooth = true) {
  hour ??= new Date().getHours()

  const options = {
    behavior: isSmooth ? 'smooth' : 'instant'
  }
  if (hour === MIDNIGHT_HOUR) {
    document.body.scrollIntoView(options)
  } else {
    timelineItemRefs.value[hour - 1].$el.scrollIntoView(options)
  }
}
</script>

<template>
  <div class="mt-7">
    <ul>
      <TimelineItem
        v-for="timelineItem in timelineItems"
        :key="timelineItem.hour"
        :activities="activities"
        :timeline-item="timelineItem"
        :activity-select-options="activitySelectOptions"
        @scroll-to-hour="scrollToHour"
        @select-activity="emit('setTimelineItemActivity', timelineItem, $event)"
        ref="timelineItemRefs"
      />
    </ul>
  </div>
</template>
