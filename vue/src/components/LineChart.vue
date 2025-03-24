<template>
  <div>
    <Line :data="data" :options="options" />
  </div>
</template>

<script>
import { Line } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  CategoryScale,
  LinearScale
);

export default {
  components: {
    Line,
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
          text: "Death Count Over Time",
        },
      },
    };

    return { options };
  },
};
</script>
