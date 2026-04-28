<script setup>
import { ref, onMounted } from "vue";
import { Chart, registerables } from "chart.js";

Chart.register(...registerables);
// eslint-disable-next-line
const props = defineProps({
  place: {
    type: Object,
  },
});

const chartCanvas = ref(null);

//creat new chart

function buildChart() {
  const hourlyData = props.place.forecast.forecastday[0].hour;

  const hours = hourlyData.map((h) => {
    const date = new Date(h.time);
    return `${date.getHours()}:00`;
  });

  const precip = hourlyData.map((hour) => hour.precip_in);

  new Chart(chartCanvas.value, {
    type: "line",
    data: {
      labels: hours,
      datasets: [
        {
          label: "Hourly Precipitation (in)",
          data: precip,
          fill: false,
          borderColor: "rgb(75, 192, 192)",
          backgroundColor: "rgba(75, 192, 192, 0.2)",
          tension: 0.3,
        },
      ],
    },
    options: {
      responsive: true,
      plugins: {
        legend: { display: true },
      },
      scales: {
        y: {
          title: { display: true, text: "inches" },
        },
      },
    },
  });
}

onMounted(() => buildChart());
</script>

<template>
  <div class="mt-6">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>
