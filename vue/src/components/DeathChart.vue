<template>
  <div>
    <h1>Leading Causes of Death in NYC</h1>
    <BarChart :data="chartData1" />
    <LineChart :data="chartData2" />
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import BarChart from "./BarChart.vue";
import LineChart from "./LineChart.vue";

export default {
  components: {
    BarChart,
    LineChart,
  },
  setup() {
    const chartData1 = ref({
      labels: [],
      datasets: [
        {
          label: "Death Count by Age Group",
          data: [],
          backgroundColor: "rgba(75, 192, 192, 0.2)",
          borderColor: "rgba(75, 192, 192, 1)",
        },
      ],
    });

    const chartData2 = ref({
      labels: [],
      datasets: [
        {
          label: "Death Count Over Time",
          data: [],
          fill: false,
          borderColor: "rgb(75, 192, 192)",
          tension: 0.1,
        },
      ],
    });
    onMounted(async () => {
      try {
        const response = await fetch(
          "https://data.cityofnewyork.us/resource/jb7j-dtam.json"
        );

        if (!response.ok) {
          throw new Error("Network response was not ok");
        }

        const rawData = await response.json();

        chartData1.value.labels = rawData.map((item) => item.age_group);
        chartData1.value.datasets[0].data = rawData.map(
          (item) => item.death_count
        );

        chartData2.value.labels = rawData.map((item) => item.year);
        chartData2.value.datasets[0].data = rawData.map(
          (item) => item.death_count
        );
      } catch (error) {
        alert("Error fetching data: " + error.message);
      }
    });

    return {
      chartData1,
      chartData2,
    };
  },
};
</script>
