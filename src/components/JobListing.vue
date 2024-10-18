<script setup>
import { ref, computed } from 'vue'
const props = defineProps({
  job: {
    type: Object,
    required: true,
  },
})
const showFullDescription = ref(false)
const truncate = computed(() => {
  let desc = props.job.description
  if (!showFullDescription.value) {
    desc = desc.substring(0, 90) + '...'
  }
  return desc
})
const toggleShow = () => {
  showFullDescription.value = !showFullDescription.value
}
</script>

<style>
/* Ensure smooth transition when expanding/collapsing description */
.description {
  max-height: 4rem; /* Set the initial collapsed height */
  overflow: hidden;
  transition:
    max-height 0.3s ease-in-out,
    opacity 0.3s ease-in-out;
  opacity: 1;
}

.description-expanded {
  max-height: 10rem; /* Large enough to hold the entire description */
}
</style>

<template>
  <div
    class="text-left bg-white rounded-xl shadow-md relative p-4 flex flex-col"
  >
    <div class="flex flex-col grow">
      <div class="text-gray-600 my-2">{{ job.type }}</div>
      <h3 class="text-xl font-bold">{{ job.title }}</h3>
      <div
        :class="[
          'description',
          { 'description-expanded': showFullDescription },
          { 'hide-opacity': !showFullDescription },
        ]"
        class="mt-2 text-gray-700"
      >
        {{ truncate }}
      </div>
      <p class="mt-2 text-center" @click="toggleShow">
        {{ showFullDescription ? 'less' : 'more' }}
      </p>
    </div>

    <div class="mt-4">
      <h3 class="text-green-500 text-lg mb-2">{{ job.salary }} / Year</h3>

      <div class="border-t border-gray-100 my-4"></div>

      <div class="flex flex-col lg:flex-row justify-between items-center">
        <div class="text-orange-700 flex items-center mb-3 lg:mb-0">
          <i class="fa-solid fa-location-dot text-lg mr-2"></i>
          {{ job.location }}
        </div>

        <RouterLink
          :to="'/jobs/' + job.id"
          class="h-[36px] bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg text-center text-sm w-full lg:w-auto"
        >
          Read More
        </RouterLink>
      </div>
    </div>
  </div>
</template>
