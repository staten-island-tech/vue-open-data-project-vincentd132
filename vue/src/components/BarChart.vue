<template>
  <div>
    <Bar :data="data" :options="options" />
  </div>
</template>

<script>
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

export default {
  components: {
    Bar,
  },
  props: {
    data: {
      type: Object,
      required: true,
    },
  },
  setup() {
    const options = {
      responsive: true,
      plugins: {
        legend: {
          position: "top",
        },
        title: {
          display: true,
          text: "Deaths by Age Group",
        },
      },
    };

    return { options };
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
