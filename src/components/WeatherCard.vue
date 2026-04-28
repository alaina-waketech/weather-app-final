<script setup>
import { ref } from "vue";
import BorderLine from "./BorderLine.vue";
import WeatherInfo from "./WeatherInfo.vue";
import WeeklyForecast from "./WeeklyForecast.vue";
import HourlyForecast from "./HourlyForecast.vue";
import HourlyPrecipitation from "./HourlyPrecipitation.vue";

//eslint-disable-next-line
defineProps({
  place: Object,
  day: Object,
});
//eslint-disable-next-line
const emit = defineEmits(["delete-place"]);
//eslint-disable-next-line
const showMore = ref(false);
//eslint-disable-next-line
const showWeekly = ref(false);
//eslint-disable-next-line
const showHourlyTemp = ref(false);
//eslint-disable-next-line
const showHourlyPrecip = ref(false);

const removePlace = (placeName) => {
  emit("delete-place", placeName);
  showMore.value = false;
};
</script>

<template>
  <div
    :class="place.current.is_day === 1 ? 'bg-day' : 'bg-night'"
    class="text-white p-10 rounded-lg shadow-lg gap-6 mb-6 relative overflow-hidden"
  >
    <!-- Location & time -->
    <div class="mb-2 flex justify-between items-center">
      <div class="flex flex-col items-start gap-2">
        <div class="flex items-center gap-2">
          <i class="fa-solid fa-location-dot"></i>
          <h1 class="text-2xl">{{ place.location.name }},</h1>
        </div>
        <div class="flex items-center gap-2">
          <h1 class="text-2xl">{{ place.location.region }},</h1>
        </div>
        <div class="flex items-center gap-2">
          <h1 class="text-2xl">
            {{ place.location.country }}
          </h1>
        </div>
      </div>

      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-clock"></i>
        <h1 class="text-3xl">
          {{ new Date(place.location.localtime).getHours() }}:{{
            new Date(place.location.localtime).getMinutes()
          }}
        </h1>
      </div>
    </div>

    <!-- current weather -->
    <div class="text-center flex-1">
      <img
        :src="place.current.condition.icon"
        alt="icon"
        width="150"
        class="mx-auto -mb-10"
      />
      <h1 class="text-9xl -mr-2 mb-2">
        {{ Math.round(place.current.temp_f) }}&deg;
      </h1>
      <p class="text-2xl">{{ place.current.condition.text }}</p>

      <p
        v-if="place.forecast?.forecastday?.length"
        class="text-center mt-5 text-lg"
      >
        {{ Math.round(place.forecast.forecastday[0].day.maxtemp_f) }}&deg; /
        {{ Math.round(place.forecast.forecastday[0].day.mintemp_f) }}&deg;
      </p>
    </div>

    <BorderLine />

    <!-- forecast -->
    <div
      v-for="(day, idx) in place.forecast.forecastday"
      :key="idx"
      class="mt-2"
    >
      <WeatherForecastDay :day="day" />
    </div>

    <!-- info -->
    <div v-show="showMore">
      <WeatherInfo
        :place="place"
        @close-info="showMore = false"
        @remove-place="removePlace(place.location.name)"
      />
    </div>

    <div v-if="showWeekly">
      <WeeklyForecast :place="place" @close-info="showWeekly = false" />
    </div>

    <div v-if="showHourlyTemp">
      <HourlyForecast :place="place" @close-info="showHourlyTemp = false" />
    </div>

    <div v-if="showHourlyPrecip">
      <HourlyPrecipitation
        :place="place"
        @close-info="showHourlyPrecip = false"
      />
    </div>

    <!-- forecast buttons -->
    <div class="flex justify-center items-center gap-1 mt-10">
      <button @click="showWeekly = true">
        Weekly Forecast <i class="fa-solid text-lg -mb-px"></i>
      </button>
      <button @click="showHourlyTemp = true">
        Hourly Temperature <i class="fa-solid text-lg -mb-px"></i>
      </button>
      <button @click="showHourlyPrecip = true">
        Hourly Precipitation <i class="fa-solid text-lg -mb-px"></i>
      </button>
      <button @click="showMore = true">
        More <i class="fa-solid fa-arrow-right text-lg -mb-px"></i>
      </button>
    </div>
  </div>
</template>

<style>
.bg-day {
  background-color: lightblue;
  background-image: url(../assets/pexels-clickerhappy-3768.jpg);
}
.bg-night {
  background-color: #39393f;
  background-image: url(../assets/pexels-stars-1869692.jpg);
}
</style>
