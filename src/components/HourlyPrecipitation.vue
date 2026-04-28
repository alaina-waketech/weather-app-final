<script setup>
import ChartHourlyRain from "./ChartHourlyRain.vue";
//eslint-disable-next-line
defineProps({
  place: Object,
});

//eslint-disable-next-line
</script>

<template>
  <div
    class="absolute bg-white/80 backdrop-blur-sm text-slate-900 inset-x-0 bottom-0 rounded-t-lg p-8"
  >
    <ChartHourlyRain :place="place" />

    <!-- Close button -->
    <div class="flex justify-end mb-6">
      <button @click="$emit('close-info')" class="p-1">
        <i class="fa-solid fa-xmark text-xl"></i>
      </button>
    </div>

    <!-- Last update -->
    <div class="flex justify-between items-center mt-6">
      <h3 class="text-slate-900/50">
        last update: {{ place.current.last_updated }}
      </h3>
    </div>
  </div>
</template>
