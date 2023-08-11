<script setup>
import { currentPage, navigate } from '@/router'
import { PAGE_TIMELINE } from '@/constants'
import { scrollToCurrentHour } from '@/timeline-items'

function handleClick() {
  currentPage.value === PAGE_TIMELINE ? scrollToCurrentHour() : navigate(PAGE_TIMELINE)
}
</script>

<template>
  <a @click="handleClick" :href="`#${PAGE_TIMELINE}`">
    <img src="../assets/img/logo.png" alt="Logo" class="h-9" />
  </a>
</template>
