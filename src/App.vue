<script setup>
import { provide, readonly } from 'vue'
import { PAGE_TIMELINE, PAGE_ACTIVITIES, PAGE_PROGRESS } from './constants'
import { generatePeriodSelectOptions } from './functions'
import {
  deleteActivity,
  createActivity,
  setActivitySecondsToComplete,
  activitySelectOptions
} from '@/activities'
import {
  setTimelineItemActivity,
  updateTimelineItemActivitySeconds,
  resetTimelineItemActivities
} from '@/timeline-items'

import { currentPage, timelineRef } from '@/router'
import * as keys from '@/keys'

import TheHeader from '@/components/TheHeader.vue'
import TheNav from '@/components/TheNav.vue'
import TheTimeline from '@/pages/TheTimeline.vue'
import TheActivities from '@/pages/TheActivities.vue'
import TheProgress from '@/pages/TheProgress.vue'

provide(keys.updateTimelineItemActivitySecondsKey, updateTimelineItemActivitySeconds)
provide(keys.setTimelineItemActivityKey, setTimelineItemActivity)
provide(keys.setActivitySecondsToCompleteKey, setActivitySecondsToComplete)
provide(keys.createActivityKey, createActivity)
provide(keys.deleteActivityKey, (activity) => {
  resetTimelineItemActivities(activity)
  deleteActivity(activity)
})

provide(keys.periodSelectOptionsKey, readonly(generatePeriodSelectOptions()))
provide(keys.activitySelectOptionsKey, readonly(activitySelectOptions)) // to keep it reactive pass without .value
</script>

<template>
  <TheHeader />
  <main class="flex flex-grow flex-col">
    <TheTimeline v-show="currentPage === PAGE_TIMELINE" ref="timelineRef" />
    <TheActivities v-show="currentPage === PAGE_ACTIVITIES" />
    <TheProgress v-show="currentPage === PAGE_PROGRESS" />
  </main>

  <TheNav />
</template>

<style scoped></style>
