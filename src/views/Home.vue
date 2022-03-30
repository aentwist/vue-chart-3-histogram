<template>
  <div class="home">
    <BarChart :chartData="chartData" :chartOptions="chartOptions" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "@vue/composition-api";
import { Chart, registerables, ChartData, ChartOptions } from "chart.js";
import { BarChart } from "vue-chart-3";

Chart.register(...registerables);

export default defineComponent({
  name: "Home",

  components: {
    BarChart,
  },

  setup() {
    const chartData: ChartData = {
      datasets: [
        {
          type: "bar",
          label: "Customers",
          data: [
            { x: 0.5, y: 3 },
            { x: 1.5, y: 2 },
            { x: 2.5, y: 3 },
            { x: 3.5, y: 0 },
            { x: 4.5, y: 1 },
          ],
          backgroundColor: "goldenrod",
          borderColor: "black",
          borderWidth: 1,
          barPercentage: 1,
          categoryPercentage: 1,
        },
        {
          type: "bar",
          label: "Sales",
          data: [
            { x: 0.5, y: 15 },
            { x: 1.5, y: 16 },
            { x: 2.5, y: 11 },
            { x: 3.5, y: 13 },
            { x: 4.5, y: 20 },
          ],
          backgroundColor: "gold",
          borderColor: "black",
          borderWidth: 1,
          barPercentage: 1,
          categoryPercentage: 1,
          yAxisID: "y2",
        },
      ],
    };
    const chartOptions: ChartOptions<"bar"> = {
      maintainAspectRatio: false,
      interaction: {
        mode: "index",
      },
      scales: {
        x: {
          type: "linear",
          offset: false,
          grid: {
            offset: false,
          },
          ticks: {
            stepSize: 1,
          },
          title: {
            display: true,
            text: "Hour",
          },
        },
        y1: {
          title: {
            display: true,
            text: "Customers",
          },
        },
        y2: {
          position: "right",
          title: {
            display: true,
            text: "Sales",
          },
        },
      },
      plugins: {
        tooltip: {
          callbacks: {
            title(items) {
              if (!items) return "";

              const x = items[0].parsed.x;

              return `Hour ${Math.floor(x)} - ${Math.ceil(x)}`;
            },
          },
        },
      },
    };

    return {
      chartData,
      chartOptions,
    };
  },
});
</script>
