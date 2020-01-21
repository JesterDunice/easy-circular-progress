<template>
  <div class="vue-circular-progress">
    <div class="circle">
      <svg :width="circleSize" :height="circleSize" class="circle__svg">
        <circle
          :cx="centralP"
          :cy="centralP"
          :r="radius"
          :style="{ 'stroke-width': strokeWidth, stroke: strokeColor }"
          class="circle__progress circle__progress--path"
        ></circle>
        <circle
          :cx="centralP"
          :cy="centralP"
          :r="radius"
          :style="fileStyl"
          class="circle__progress circle__progress--fill"
        ></circle>
      </svg>

      <div class="percent">
        <slot>
          <span class="percent__int">{{ int }}</span>
          <span class="dot">.</span>
          <span class="percent__dec">{{ dec }}</span>
          <span class="percent_sign">%</span>
        </slot>
      </div>
    </div>
    <slot name="footer"></slot>
  </div>
</template>
<script>
export default {
  name: 'easy-circular-progress',
  props: {
    strokeWidth: {
      type: Number,
      default: 4
    },
    radius: {
      type: Number,
      default: 38
    },
    transitionDuration: {
      type: Number,
      default: 1000
    },
    strokeColor: {
      type: String,
      default: "#aaff00"
    },
    value: {
      validator: function(value) {
        // should be a number and less or equal than 100
        return !Number.isNaN(Number(value)) && Number(value) <= 100;
      },
      default: "0.0"
    }
  },
  data() {
    return {
      int: 0,
      dec: "00"
    };
  },
  computed: {
    circumference() {
      return this.radius * Math.PI * 2;
    },
    fileStyl() {
      return {
        strokeDashoffset: this.offset,
        "--initialStroke": this.circumference,
        "--transitionDuration": `${this.transitionDuration}ms`,
        "stroke-width": this.strokeWidth,
        stroke: this.strokeColor
      };
    },
    circleSize() {
      return (this.radius + this.strokeWidth) * 2;
    },
    centralP() {
      return this.circleSize / 2;
    },
    offset() {
      return (this.circumference * (100 - this.value)) / 100;
    }
  }
};
</script>
<style lang="scss">
@import "index";
</style>
