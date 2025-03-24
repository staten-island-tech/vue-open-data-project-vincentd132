<template>
  <div id="home">
    <!-- Title in the top center -->
    <h1 class="chart-title">Death Charts Visualization</h1>

    <!-- Buttons to toggle charts, centered below the title -->
    <div class="button-container">
      <button @click="showChart('death')">Show Death Chart</button>
      <button @click="showChart('line')">Show Line Chart</button>
      <button @click="showChart('bar')">Show Bar Chart</button>
    </div>

    <!-- Conditional rendering of charts based on currentChart value -->
    <DeathChart v-if="currentChart === 'death'" :data="deathChartData" />
    <LineChart v-if="currentChart === 'line'" :data="lineChartData" />
    <BarChart v-if="currentChart === 'bar'" :data="barChartData" />
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import DeathChart from "../components/DeathChart.vue";
import LineChart from "../components/LineChart.vue";
import BarChart from "../components/BarChart.vue";

export default {
  components: {
    DeathChart,
    LineChart,
    BarChart,
  },
  setup() {
    const currentChart = ref("death"); // Default chart to show on load

    const deathChartData = ref({});
    const lineChartData = ref({});
    const barChartData = ref({});

    onMounted(() => {
      fetch("https://data.cityofnewyork.us/resource/jb7j-dtam.json")
        .then((response) => response.json())
        .then((data) => {
          deathChartData.value = processDeathChart(data);
          lineChartData.value = processLineData(data);
          barChartData.value = processBarData(data);
        })
        .catch((error) => {
          console.error("Error fetching API data:", error);
        });
    });

    const processDeathChart = (data) => {
      return {
        labels: data.map((item) => item.age_group),
        datasets: [
          {
            label: "Death Count by Age Group",
            data: data.map((item) => item.count),
            backgroundColor: "rgba(255, 99, 132, 0.2)",
            borderColor: "rgba(255, 99, 132, 1)",
            borderWidth: 1,
          },
        ],
      };
    };

    const processLineData = (data) => {
      return {
        labels: data.map((item) => item.year),
        datasets: [
          {
            label: "Deaths Over Time",
            data: data.map((item) => item.deaths),
            borderColor: "rgba(75,192,192,1)",
            backgroundColor: "rgba(75,192,192,0.2)",
            fill: true,
          },
        ],
      };
    };

    const processBarData = (data) => {
      return {
        labels: data.map((item) => item.age_group),
        datasets: [
          {
            label: "Deaths by Age Group",
            data: data.map((item) => item.count),
            backgroundColor: "rgba(255, 99, 132, 0.2)",
            borderColor: "rgba(255, 99, 132, 1)",
            borderWidth: 1,
          },
        ],
      };
    };

    // Function to set the chart to be displayed
    const showChart = (chartType) => {
      currentChart.value = chartType;
    };

    return {
      currentChart,
      deathChartData,
      lineChartData,
      barChartData,
      showChart,
    };
  },
};
</script>

<style scoped>
/* Center the title at the top */
.chart-title {
  text-align: center;
  font-size: 2em;
  margin-top: 20px;
  font-family: "Arial", sans-serif;
}

/* Center the buttons below the title */
.button-container {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

/* Button styling */
button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #45a049;
}

/* Optional: Add some margin to the charts to create space */
.chart-container {
  margin-top: 40px;
}
</style>
