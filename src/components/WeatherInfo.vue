<script setup>
//eslint-disable-next-line
defineProps({
  place: Object,
});
</script>

<template>
  <div
    class="absolute bg-white/80 backdrop-blur-sm text-slate-900 inset-x-0 bottom-0 rounded-t-lg p-8"
  >
    <!-- Close button -->
    <div class="flex justify-end mb-10">
      <button @click="$emit('close-info')" class="p-1">
        <i class="fa-solid fa-xmark text-xl"></i>
      </button>
    </div>

    <div class="flex items-center justify-between gap-6 mb-20">
      <!-- Wind speed -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-wind mb text-2xl"></i>
        <p class="text-xl font-bold">{{ place.current.wind_mph }}</p>
        <p>wind</p>
      </div>
      <!-- Humidity level -->
      <div class="text-center flex-1">
        <i class="fa-solid fa-droplet mb text-2xl"></i>
        <p class="text-xl font-bold">{{ place.current.humidity }}%</p>
        <p>humidity</p>
      </div>
    </div>

    <!-- Last update and delete -->
    <div class="flex justify-between items-center">
      <h3 class="text-slate-900/50">
        last update: {{ place.current.last_updated }}
      </h3>
      <button @click="$emit('remove-place')">
        Delete This Card
        <i class="fa-solid fa-trash"></i>
      </button>
    </div>
  </div>
</template>
