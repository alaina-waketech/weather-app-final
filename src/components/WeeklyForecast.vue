<script setup>
//eslint-disable-next-line

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

    <table class="w-full">
      <tbody>
        <tr v-for="day in place.forecast.forecastday" :key="day.date">
          <!-- day of the week -->
          <td class="w-1/4">
            {{
              new Date(day.date + "T00:00:00").toLocaleDateString("en-us", {
                weekday: "long",
              })
            }}
          </td>

          <!-- icon -->
          <td class="w-1/4">
            <img
              :src="day.day.condition.icon"
              alt="icon"
              width="30"
              class="mx-auto"
            />
          </td>
          <!--Weather Condtions-->
          <td class="w-1/4">
            {{ day.day.condition.text }}
          </td>

          <!-- high/low temp -->
          <td class="w-1/4 text-right">
            {{ Math.round(day.day.maxtemp_f) }}&deg; /
            {{ Math.round(day.day.mintemp_f) }}&deg;
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Last update and delete -->
    <div class="flex justify-between items-center mt-6">
      <h3 class="text-slate-900/50">
        last update: {{ place.current.last_updated }}
      </h3>
    </div>
  </div>

  <ChartWeekHiLo />
</template>
