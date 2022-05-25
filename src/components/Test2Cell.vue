<!-- Eksempel på komponent, som bruger vue-multiselect med scoped styling -->
<template>
  <span>
    <div class="headlineRow">
      <figure class="m-0 mr-3" style="width: 32px">
        <IconValue v-if="currentSection.id == 'value'" />
        <IconRessources v-else-if="currentSection.id == 'resources'" />
        <IconCustomers v-else-if="currentSection.id == 'customers'" />
        <IconSales v-else-if="currentSection.id == 'sales'" />
        <IconScales v-else />
      </figure>
      <p class="h4 m-0">
        {{ currentSection.pdfHeadline ? currentSection.pdfHeadline : currentSection.headline }}
      </p>
    </div>
    <div
      v-for="(question, questionIndex) in currentSection.questions"
      :key="question.name"
      :class="`questionRow ${questionIndex < currentSection.questions.length - 1 ? 'dotted' : ''}`"
    >
      <p class="my-3">{{ question.label }}</p>
      <div class="valueContainer ml-4">
        <p class="h5 mt-0 mb-2 align-text-center">{{ values[question.name] }}</p>
        <GradientLine :percentage="(values[question.name] / 10) * 100" :width="20" style="flex-shrink: 0" />
      </div>
    </div>
  </span>
</template>

<script lang="ts">
import GradientLine from './GradientLine.vue';
import IconScales from './IconScales.vue';
import IconValue from './IconValue.vue';
import IconRessources from './IconRessources.vue';
import IconCustomers from './IconCustomers.vue';
import IconSales from './IconSales.vue';

export default {
  name: 'Test2Cell',
  components: { GradientLine, IconScales, IconValue, IconRessources, IconCustomers, IconSales },
  props: {
    rowIndex: { type: Number, required: true },
    colIndex: { type: Number, required: true },
    steps: { type: Array, required: true },
    values: { type: Object, required: true }
  },
  computed: {
    currentSection() {
      const section = this.steps.find((step: any) => step.pdfRowIndex === this.rowIndex && step.pdfColIndex === this.colIndex);
      return section;
    }
  }
};
</script>

<style lang="scss" scoped>
>>> {
  p {
    font-size: 10px;
    line-height: 13px;
  }

  .h4,
  .h5 {
    font-size: 11px;
    line-height: 14px;
  }

  .questionRow {
    display: flex;
    align-items: center;
  }

  .headlineRow {
    display: flex;
    align-items: center;
  }

  .dotted {
    border-bottom: 1px #c8c8c8 dotted !important;
  }
}
</style>
