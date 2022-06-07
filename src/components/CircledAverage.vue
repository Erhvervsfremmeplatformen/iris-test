<!-- Eksempel på komponent, som bruger vue-multiselect med scoped styling -->
<template>
  <div
    class="circled-average"
    :style="`background-image: linear-gradient(${degrees}deg, ${color} 50%, transparent 50%), linear-gradient(0deg, #dfdfdf 50%, ${color} 50%)`"
  >
    <span class="inner-circle" :style="`border-color: ${color}`">
      {{ value }}
    </span>
  </div>
</template>

<script lang="ts">
export default {
  name: 'CircledAverage',
  props: {
    value: { type: [Number, String], required: true },
    percentage: { type: Number, required: true }
  },
  computed: {
    degrees() {
      return this.percentage * 3.6;
    },
    color() {
      if (this.percentage <= 49) {
        return '#26C223';
      }
      if (this.percentage <= 69) {
        return '#FFCB11';
      }
      if (this.percentage <= 89) {
        return '#FF9900';
      }
      return '#E22424';
    }
  }
};
</script>

<style lang="scss" scoped>
.circled-average {
  position: relative;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  margin-bottom: 1rem;
  margin-left: -8px;
  margin-top: 24px;

  .inner-circle {
    width: calc(100% - 8px);
    height: calc(100% - 8px);
    border-radius: inherit;
    position: absolute;
    left: 4px;
    top: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #ffffff;
  }

  .inner-line {
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    background: red;
    border-radius: inherit;
  }
}
</style>
