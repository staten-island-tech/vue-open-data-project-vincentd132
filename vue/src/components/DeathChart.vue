<template>
  <div class="chart-container">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script>
import { onMounted, ref, watch, onBeforeUnmount } from "vue";
import {
  Chart,
  BarController,
  BarElement,
  CategoryScale,
  LinearScale,
  Title,
  Tooltip,
  Legend,
} from "chart.js";

Chart.register(
  BarController,
  BarElement,
  CategoryScale,
  LinearScale,
  Title,
  Tooltip,
  Legend
);

export default {
  props: {
    data: Object,
  },
  setup(props) {
    const chartCanvas = ref(null);
    let chartInstance = null;

    const createChart = () => {
      if (chartInstance) {
        chartInstance.destroy();
      }

      console.log("DeathChart received data:", props.data);

      if (chartCanvas.value && props.data && props.data.labels.length) {
        chartInstance = new Chart(chartCanvas.value, {
          type: "bar",
          data: props.data,
          options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
              x: {
                ticks: {
                  autoSkip: false,
                  maxRotation: 45,
                  minRotation: 45,
                },
              },
              y: {
                beginAtZero: true,
              },
            },
            plugins: {
              legend: { position: "top" },
            },
          },
        });
      } else {
        console.warn("DeathChart has no data to display");
      }
    };

    onMounted(createChart);

    watch(
      () => props.data,
      (newData) => {
        if (newData && newData.labels.length) {
          createChart();
        }
      },
      { deep: true }
    );

    onBeforeUnmount(() => {
      if (chartInstance) {
        chartInstance.destroy();
      }
    });

    return {
      chartCanvas,
    };
  },
};
</script>

<style scoped>
.chart-container {
  width: 100%;
  max-width: 1200px;
  height: 600px;
}
</style>
