<template>
  <div class="histogram-chart">
    <h1>Average Customers & Sales vs. Hour of Day</h1>
    <canvas id="myChart" ref="ctx"></canvas>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, Ref, ref } from "@vue/composition-api";
import { Chart, registerables, ChartConfiguration } from "chart.js";

Chart.register(...registerables);

export default defineComponent({
  name: "HistogramChart",

  setup() {
    const ctx: Ref<null | HTMLCanvasElement> = ref(null);

    const data: ChartConfiguration = {
      type: "bar",
      data: {
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
      },
      options: {
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
      },
    };

    onMounted(() => {
      if (ctx.value) {
        new Chart(ctx.value, data);
      }
    });

    return {
      ctx,
    };
  },
});
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.histogram-chart {
  padding: 3rem;
  padding-top: 0;
  width: 100%;
  height: calc(100vh - 7rem);
  position: relative;
}

h1 {
  height: 7rem;
  display: grid;
  place-content: center;
}
</style>
