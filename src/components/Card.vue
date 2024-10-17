<script setup>
defineProps({
  bgColor: {
    type: String,
    default: 'bg-gray-50',
  },
})
</script>

<template>
  <div :class="[bgColor]" class="p-6 rounded-lg shadow-md">
    <slot></slot>
  </div>
</template>
