<template>
  <div class="chart-container">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script>
import { onMounted, ref, watch } from "vue";
import { Chart } from "chart.js";

export default {
  props: {
    data: Object,
  },
  setup(props) {
    const chartCanvas = ref(null);
    let chart = null;

    onMounted(() => {
      if (chartCanvas.value && props.data) {
        chart = new Chart(chartCanvas.value, {
          type: "line",
          data: props.data,
          options: {
            responsive: true,
            plugins: {
              legend: {
                position: "top",
              },
            },
          },
        });
      }
    });

    watch(
      () => props.data,
      (newData) => {
        if (chart) {
          chart.data = newData;
          chart.update();
        }
      }
    );

    return {
      chartCanvas,
    };
  },
};
</script>

<style scoped>
.chart-container {
  width: 100%;
  height: 500px;
}
</style>
