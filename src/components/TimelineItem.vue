<script setup>
import { inject } from 'vue'

import { isTimelineItemValid, isUndefined } from '@/validators'
import BaseSelect from '@/components/BaseSelect.vue'
import { setTimelineItemActivityKey, activitySelectOptionsKey } from '@/keys'

import TimelineHour from '@/components/TimelineHour.vue'
import TimelineStopWatch from '@/components/TimelineStopWatch.vue'

defineProps({
  timelineItem: {
    required: true,
    type: Object,
    validator: isTimelineItemValid
  }
})

const emit = defineEmits({
  scrollToHour: isUndefined
})

const setTimelineItemActivity = inject(setTimelineItemActivityKey)
const activitySelectOptions = inject(activitySelectOptionsKey)
</script>

<template>
  <li class="relative flex flex-col gap-2 border-t border-gray-200 px-4 py-10">
    <TimelineHour :hour="timelineItem.hour" @click.prevent="emit('scrollToHour')" />
    <BaseSelect
      :placeholder="'Rest'"
      :selected="timelineItem.activityId"
      :options="activitySelectOptions"
      @select="setTimelineItemActivity(timelineItem, $event)"
    />
    <TimelineStopWatch :timeline-item="timelineItem" />
  </li>
</template>
