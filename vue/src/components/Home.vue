<template>
  <div id="home" class="home-container">
    <h1 class="chart-title">
      {{
        currentChart === "death"
          ? "Death Chart"
          : currentChart === "line"
          ? "Line Chart"
          : "Bar Chart"
      }}
    </h1>

    <div class="button-container">
      <button @click="showChart('death')">Show Death Chart</button>
      <button @click="showChart('line')">Show Line Chart</button>
    </div>

    <div v-if="isLoading">Loading data...</div>

    <DeathChart v-if="currentChart === 'death'" :data="deathChartData" />
    <LineChart v-if="currentChart === 'line'" :data="lineChartData" />
    <BarChart v-if="currentChart === 'bar'" :data="barChartData" />
  </div>
</template>

<script>
import { ref, onMounted, watch } from "vue";
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
    const currentChart = ref("death");
    const isLoading = ref(true);
    const deathChartData = ref({});
    const lineChartData = ref({});
    const barChartData = ref({});

    watch(currentChart, (newValue) => {
      console.log("currentChart changed to:", newValue);
    });

    onMounted(() => {
      fetch("https://data.cityofnewyork.us/resource/jb7j-dtam.json")
        .then((response) => response.json())
        .then((data) => {
          console.log("Fetched data:", data);
          isLoading.value = false;
          deathChartData.value = processDeathChart(data);
          lineChartData.value = processLineData(data);
          barChartData.value = processBarData(data);
        })
        .catch((error) => {
          console.error("Error fetching data:", error);
          isLoading.value = false;
        });
    });

    const processDeathChart = (data) => {
      if (!data || data.length === 0) {
        console.warn("No data available for DeathChart");
        return { labels: [], datasets: [] };
      }

      const causeGroups = {};

      data.forEach((item) => {
        const cause = item.leading_cause || "Unknown";
        const deaths = parseInt(item.deaths, 10) || 0;

        if (!causeGroups[cause]) {
          causeGroups[cause] = 0;
        }

        causeGroups[cause] += deaths;
      });

      console.log("Processed Death Chart Data:", causeGroups);

      return {
        labels: Object.keys(causeGroups),
        datasets: [
          {
            label: "Total Deaths by Leading Cause",
            data: Object.values(causeGroups),
            backgroundColor: "rgba(255, 99, 132, 0.6)",
            borderColor: "rgba(255, 99, 132, 1)",
            borderWidth: 1,
          },
        ],
      };
    };
    const processLineData = (data) => {
      if (!data || data.length === 0) {
        console.warn("No data available for LineChart");
        return { labels: [], datasets: [] };
      }

      const labels = [...new Set(data.map((item) => item.year))].sort();

      const deaths = labels.map((year) =>
        data
          .filter((item) => item.year === year)
          .reduce((acc, item) => acc + (parseInt(item.deaths, 10) || 0), 0)
      );

      return {
        labels,
        datasets: [
          {
            label: "Total Deaths Over Time",
            data: deaths,
            borderColor: "rgba(75,192,192,1)",
            backgroundColor: "rgba(75,192,192,0.2)",
            fill: true,
          },
        ],
      };
    };

    const processBarData = (data) => {
      if (!data || data.length === 0) {
        console.warn("No data available for Race/Ethnicity Chart");
        return { labels: [], datasets: [] };
      }

      const raceGroups = {};

      data.forEach((item) => {
        const race = item.race_ethnicity || "Unknown";
        const deaths = parseInt(item.deaths, 10) || 0;

        if (!raceGroups[race]) {
          raceGroups[race] = 0;
        }

        raceGroups[race] += deaths;
      });

      const labels = Object.keys(raceGroups);
      const counts = Object.values(raceGroups);

      return {
        labels,
        datasets: [
          {
            label: "Deaths by Race/Ethnicity",
            data: counts,
            backgroundColor: [
              "rgba(255, 99, 132, 0.6)",
              "rgba(54, 162, 235, 0.6)",
              "rgba(255, 206, 86, 0.6)",
              "rgba(75, 192, 192, 0.6)",
              "rgba(153, 102, 255, 0.6)",
            ],
            borderColor: "rgba(0, 0, 0, 0.2)",
            borderWidth: 1,
          },
        ],
      };
    };
    const showChart = (chartType) => {
      currentChart.value = chartType;
    };

    return {
      currentChart,
      isLoading,
      deathChartData,
      lineChartData,
      barChartData,
      showChart,
    };
  },
};
</script>

<style scoped>
.home-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
  padding: 20px;
}

.chart-title {
  font-size: 2.5rem;
  margin-bottom: 20px;
  text-align: center;
}

.button-container {
  display: flex;
  justify-content: center;
  gap: 25px;
  margin-top: 20px;
  width: 100%;
}

.button-container button {
  padding: 15px 25px;
  font-size: 18px;
  cursor: pointer;
  border: none;
  background-color: #4caf50;
  color: white;
  transition: background-color 0.3s ease;
}

.button-container button:hover {
  background-color: #45a049;
}

.chart-container {
  width: 80%;
  height: 600px;
  margin-top: 30px;
}
</style>
