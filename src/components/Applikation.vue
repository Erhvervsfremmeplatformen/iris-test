<template>
  <div class="applikation-container">
    <div v-if="isLoading" class="spinner" aria-label="Henter indhold" />
    <div v-else class="row">
      <div class="col-lg-9">
        <p class="h6">Test</p>
        <h1>Hvordan er presset på din forretningsmodel?</h1>
        <p class="font-lead">
          Svar på en række udsagn om din virksomhed inden for 6 områder og få en indikation af, hvordan presset er på din forretningsmodel.
        </p>
        <div class="form-group">
          <fieldset>
            <legend class="h5">Vælg din virksomheds branche og start testen</legend>
            <ul class="nobullet-list">
              <li v-for="(industry, index) of industries" :key="index">
                <input :id="`radio-${index}`" type="radio" name="radio" :value="industry" class="form-radio radio-large" />
                <label :id="`form-label-radio-${index}`" :for="`radio-${index}`" class="">{{ industry }} </label>
              </li>
            </ul>
          </fieldset>
        </div>
      </div>
      <div class="col-lg-9">
        <button class="button button-primary">Start testen</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import axios from 'axios';
import SimpleForm from 'vue-simpleform';
import VueApexCharts from 'vue-apexcharts';

@Component({
  name: 'Applikation',
  components: {
    SimpleForm,
    apexchart: VueApexCharts
  }
})
export default class Applikation extends Vue {
  response = {};
  private error = {};
  isLoading = false;
  currentStep = 1;
  industries = [
    'Industri',
    'Bygge og anlæg',
    'Handel',
    'Transport',
    'Videnservice',
    'Kommunikation og reklame',
    'Operationel service',
    'Oplevelseserhverv',
    'Andet (angiv hvilket)'
  ];

  mounted() {
    this.isLoading = true;
    this.callExternalApi();
  }

  private async callExternalApi() {
    const data = JSON.stringify({
      answers: {
        value1: 10,
        value2: 8,
        value3: 8,
        value4: 7,
        customers1: 3,
        customers2: 5,
        customers3: 5,
        customer4: 10,
        sales1: 8,
        sales2: 10,
        sales3: 9,
        sales4: 9,
        resources1: 10,
        resources2: 10,
        resources3: 7,
        resources4: 7,
        valuecustomer1: 9,
        valuecustomer2: 8,
        valuecustomer3: 6,
        valuecustomer4: 9,
        customersales1: 9,
        customersales2: 9,
        customersales3: 9,
        customersales4: 9,
        salesresources1: 9,
        salesresources2: 10,
        salesresources3: 10,
        salesresources4: 10,
        resourcesvalue1: 9,
        resourcesvalue2: 8,
        resourcesvalue3: 7,
        resourcesvalue4: 6
      },
      industry: 'Industri'
    });
    console.log(data);
    axios
      .post('http://128.199.7.8/api/suggestions', data, {
        headers: {
          'Content-Type': 'application/json'
        }
      })
      .then((rsp: any) => {
        console.log(rsp.data);
        this.isLoading = false;

        if (!rsp.data.error) {
          this.isLoading = false;
          this.response = rsp.data;
        }
      })
      .catch((error: any) => {
        console.log(error);
        this.isLoading = false;
        this.error = 'Noget gik galt. Prøv venligst igen.';
      });
  }
}
</script>
<style lang="scss" scoped></style>
