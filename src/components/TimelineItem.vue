<script setup>
import {
  isActivityValid,
  isTimelineItemValid,
  validateSelectOptions,
  validateActivities,
  isHourValid
} from '@/validators'
import { NULLABLE_ACTIVITY } from '@/constants'
import BaseSelect from '@/components/BaseSelect.vue'

import TimelineHour from '@/components/TimelineHour.vue'
import TimelineStopWatch from '@/components/TimelineStopWatch.vue'

const props = defineProps({
  timelineItem: {
    required: true,
    type: Object,
    validator: isTimelineItemValid
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
  }
})

const emit = defineEmits({
  selectActivity: isActivityValid,
  scrollToHour: isHourValid()
})

function findActivityById(id) {
  return props.activities.find((activity) => activity.id === id) || NULLABLE_ACTIVITY
}

function selectActivity(id) {
  emit('selectActivity', findActivityById(id))
}
</script>

<template>
  <li class="relative flex flex-col gap-2 border-t border-gray-200 px-4 py-10">
    <TimelineHour
      :hour="timelineItem.hour"
      @click.prevent="emit('scrollToHour', timelineItem.hour)"
    />
    <BaseSelect
      :placeholder="'Rest'"
      :selected="timelineItem.activityId"
      :options="activitySelectOptions"
      @select="selectActivity"
    />
    <TimelineStopWatch :timeline-item="timelineItem" />
  </li>
</template>
