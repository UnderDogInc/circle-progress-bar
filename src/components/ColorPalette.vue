<template>
  <div class="progress__wrapper">
    <svg height="240" width="240" class="progress" xmlns="http://www.w3.org/2000/svg" viewBox="-10 -10 50 50">
      <g stroke-width="6" class="progress__group" transform="translate(-3, -3)">
        <circle r="16" cx="18" cy="18" stroke="#282F37" fill="#505B69" />
        <circle
            :id="'progress-path-' + componentId"
            class="progress__path"
            r="16"
            cx="18"
            cy="18"
            :stroke="value > targetValue ? '#298D40' : '#ff8c00'"
            :stroke-dasharray="computedStrokeDasharray"
            stroke-linecap="round"
            fill="transparent"
            :transform="'rotate(-90 18 18)'"
        />
        <circle
            class="progress__value current"
            :cx="circlePosition(currentValue).x"
            :cy="circlePosition(currentValue).y"
            r="2"
            fill="#3E4B5C"
        />
        <text
            :x="circlePosition(currentValue).x"
            :y="circlePosition(currentValue).y"
            dy="0.35em"
            :fill="value > targetValue ? '#298D40' : '#ff8c00'"
            font-weight="bold"
            font-size="2px"
            class="circle-text"
        >
          {{ currentValue }}
        </text>
        <text
            v-if="value < targetValue"
            :transform="rotateTargetValue"
            :x="circlePosition(targetValue).x"
            :y="circlePosition(targetValue).y"
            dy="0.35em"
            fill="#778AA6"
            font-size="1px"
            class="circle-text target-value"
        >
          {{ targetValue }}
        </text>
        <text
            v-if="value < maxValue"
            :x="circlePosition(maxValue).x - 5"
            y="-1"
            dy="1.5em"
            fill="#778AA6"
            font-size="3px"
            class="max-value"
        >
          {{ maxValue }}
        </text>
      </g>
    </svg>
    <div class="progress-main">
      <span v-if="value < targetValue" class="progress-main__text">
        Ещё продать на завтра
      </span>
      <span :style="{ color: value > targetValue ? '#298D40' : '#ff8c00' }" class="progress-main__count">
        {{ value < targetValue ? targetValue - value : value }}
        <small class="progress-main__time">ч</small>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    maxValue: {
      type: Number,
      default: 108,
    },
    value: {
      type: Number,
      default: 36,
    },
    targetValue: {
      type: Number,
      default: 80,
    }
  },
  data() {
    return {
      componentId: Math.random().toString(36).substring(7)
    };
  },
  computed: {
    rotateTargetValue() {
      const rotate = this.targetValue < this.maxValue / 2 ? this.targetValue : -this.targetValue;
      return `rotate(${rotate + 10} ${this.circlePosition(this.targetValue).x} ${this.circlePosition(this.targetValue).y})`;
    },
    currentValue() {
      return this.value;
    },
    computedStrokeDasharray() {
      const ratio = this.currentValue / this.maxValue;
      return `${ratio * 100} 100`;
    }
  },
  methods: {
    circlePosition(value) {
      const angle = (value / this.maxValue) * 360 - 90;
      const x = 18 + 16 * Math.cos(angle * Math.PI / 180);
      const y = 18 + 16 * Math.sin(angle * Math.PI / 180);
      return { x, y };
    }
  }
};
</script>

<style>
:root {
  --orange: #ff8c00;
  --green: #298D40;
  --text-color: #B5C3D7;
}

.progress {
  height: 240px;
  width: 240px;
  position: relative;
}

.progress__wrapper {
  position: relative;
  width: min-content;
  height: min-content;
}

.progress-main {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.progress-main__count {
  font-size: 24px;
  font-weight: bold;
}

.progress-main__time {
  font-size: 12px;
  color: var(--text-color);
  font-weight: normal;
}

.progress-main__text {
  max-width: 90px;
  font-size: 12px;
  color: var(--text-color);
}

.circle-text {
  text-anchor: middle;
}

.target-value {
  font-size: 4px;
}

.max-value {
  text-anchor: middle;
  dominant-baseline: middle;
}
</style>

