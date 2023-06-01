<template>
  <div class="progress-container">
    <svg class="chart" viewBox="0 0 32 32" width="120" height="120">
      <circle class="chart-background" :r="radius" :cx="center" :cy="center" />
      <circle
        class="chart-progress"
        :r="radius"
        :cx="center"
        :cy="center"
        :stroke-dasharray="dasharray"
        :stroke-dashoffset="dashoffset"
      />
      <text class="chart-status" :x="center" :y="center + 4">{{ status }}</text>
      <text class="chart-value" :x="center" :y="center - 6">{{ value }}%</text>
    </svg>
    <div class="slider">
      <input
        type="range"
        min="0"
        max="100"
        v-model="value"
        @input="updateProgress"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "ProgressChart",
  data() {
    return {
      radius: 16,
      center: 16,
      value: 0,
      status: "未知",
    };
  },
  computed: {
    dasharray() {
      const circumference = 2 * Math.PI * this.radius;
      return `${circumference} ${circumference}`;
    },
    dashoffset() {
      const circumference = 2 * Math.PI * this.radius;
      const percent = this.value / 100;
      return circumference * (1 - percent);
    },
  },
  methods: {
    updateProgress() {
      if (this.value === 0) {
        this.status = "未知";
      } else if (this.value <= 50) {
        this.status = "正常";
      } else {
        this.status = "报警";
      }
    },
  },
};
</script>

<style scoped>
.progress-container {
  display: flex;
  flex-direction: column;
}

.chart {
  margin: 0 auto;
}

.chart-background,
.chart-progress {
  fill: none;
  stroke: #ddd;
  stroke-width: 1.5;
}

.chart-progress {
  stroke: #159afa;
  transition: stroke-dashoffset 0.5s;
}

.chart-status,
.chart-value {
  font-size: 8px;
  text-anchor: middle;
}

.slider {
  width: 120px;
  margin: 0 auto;
}

input[type="range"] {
  margin: 0;
  padding: 0;
  height: 5px;
  -webkit-appearance: none;
  background-color: #ddd;
  border-radius: 2.5px;

  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 10px;
    height: 10px;
    background-color: #159afa;
    border-radius: 50%;
  }
}
</style>
