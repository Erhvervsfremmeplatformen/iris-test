<template>
  <div class="applikation-container">
    <div class="row">
      <div v-if="isLoading" class="spinner" aria-label="Henter indhold" />

      <!-- Frontpage start -->
      <template v-else-if="currentSection === 'frontpage'">
        <div class="col-12 align-text-center">
          <h1>
            Test og styrk din virksomheds forretningsmodel
            <div class="h2 m-4">med diagnoseværktøj i to dele</div>
            <p class="font-lead" style="max-width: none">Vælg en af af de to tests nedenfor</p>
            <div class="row" style="justify-content: center">
              <div class="m-2">
                <button class="button button-primary" @click.prevent="currentSection = 'test1'">Vurder presset på din forretningsmodel</button>
              </div>
              <div class="m-2">
                <button class="button button-primary" @click.prevent="currentSection = 'test2'">Få forbedringer til din forretningsmodel</button>
              </div>
            </div>
          </h1>
        </div>
        <div class="col-12 bg-normal mt-9 p-7">
          <div class="row">
            <div class="col-6">
              <h2 class="mt-4">Hvorfor teste din forretningsmodel?</h2>
              <p>
                Der kan være mange årsager til, at en forretningsmodel kommer under pres, eller at der opstår nye muligheder i markedet, der gør det
                relevant at styrke eller justere forretningsmodelen. Formålet med de to test ser at give dig en indikation af, om der kan være behov
                for at efterse hele eller dele af din virksomheds forretningsmodel.
              </p>
              <h2>Hvem kan tage testen?</h2>
              <p>
                De to tests er målrettet små og mellemstore virksomheder. Den kan besvares af ledelsen eller medarbejdere, og kan med fordel besvares
                af flere fra samme virksomhed, så svarene eventuelt kan sammenlignes internt.
              </p>
              <p>
                Testene baserer sig på besvarelser fra en stor undersøgelse blandt mindre og mellemstore virksomheder, der succesfuldt har forandret
                eller styrket deres forretningsmodel.
              </p>
            </div>
            <div class="col-6">
              <div class="card">
                <div class="card-header px-7">
                  <h2>Sådan fungerer testen</h2>
                </div>
                <div class="card-text px-7">
                  <p>
                    Testen baserer sig på en større undersøgelse blandt små og mellemstore virksomheder, der succesfuldt har forandret eller styrket
                    hele eller dele af forretningen. Den viser, at drivkræfterne bag forretningsmodeludvikling typisk falder inden for seks
                    hovedområder:
                  </p>
                  <ul>
                    <li v-for="(theme, index) of sections[0].steps" :key="index">{{ theme.headline }}</li>
                  </ul>
                  <p>
                    Derfor beder vi dig i den første test om at stilling til en række udsagn knyttet til disse hovedområder. I den anden test beder vi
                    dig om at tage stilling til de fire dele i forretningsmodellen – 1) virksomhedens værditilbud, 2) kunder, 3) Salg 4) og
                    kommunikation, Ressourcer, partnere og processer, samt samspillet mellem dem
                  </p>
                </div>
              </div>
            </div>
            <div class="col-6">
              <div class="d-flex flex-column bg-alternative">
                <img src="/img/test1.png" alt="billede" />
                <div class="card w-percent-100">
                  <div class="card-text">
                    <h3>Vurder presset på din forretningsmodel</h3>
                    <p>
                      Der er flere ting der spiller ind, når en forretningsmodel kommer under pres. Tag testen og få en indikation af, om der kan være
                      behov for at efterse hele eller dele af din forretningsmodel.
                    </p>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-6">
              <div class="d-flex flex-column bg-alternative">
                <img src="/img/test2.png" alt="billede" />
                <div class="card w-percent-100">
                  <div class="card-text">
                    <h3>Juster din forretningsmodel</h3>
                    <p>
                      For at udvikle en god forretningsmodel kræver det balance i modellens fire elementer, samt samspillet mellem dem. Tag testen og
                      få inspiration til hvordan din forretningsmodel kan justeres eller forbedres.
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
      <!-- Frontpage end -->

      <!-- test 1 + 2 loop -->
      <div v-for="(section, sectionIndex) of sections" v-else :key="sectionIndex" class="col-lg-9">
        <template v-if="currentSection === section.id">
          <SimpleForm :value="initialValues" :validate="validate" @submit="handleSubmit">
            <template slot-scope="{ values, input, blur, validate, setValue, handleSubmit }">
              <p class="h6">Test {{ sectionIndex + 1 }}</p>

              <h2 class="h1">{{ section.headline }}</h2>
              <p v-if="section.description" class="font-lead">
                {{ section.description }}
              </p>

              <div v-if="currentStep > 0 && currentStep < section.steps.length" class="overflow-menu overflow-menu--open-right">
                <button
                  class="button-overflow-menu js-dropdown"
                  data-js-target="overflow5"
                  aria-haspopup="true"
                  aria-expanded="false"
                  id="overflow-button"
                >
                  Trin {{ currentStep }} af {{ section.steps.length - 1 }}
                  <svg class="icon-svg" aria-hidden="true" focusable="false"><use xlink:href="#arrow-drop-down"></use></svg>
                </button>
                <div id="overflow5" class="overflow-menu-inner" aria-hidden="true">
                  <nav>
                    <ul class="overflow-list sidenav-list" role="menu">
                      <li role="none">
                        <a href="#" role="menuitem">
                          1. Trin 1 <svg class="icon-svg" aria-hidden="true" focusable="false"><use xlink:href="#arrow-drop-down"></use></svg>
                          <span class="sidenav-icon">
                            <svg class="icon-svg" aria-hidden="true" focusable="false" tabindex="-1"><use xlink:href="#done"></use></svg>
                          </span>
                        </a>
                      </li>
                      <li role="none" class="active current">
                        <a href="#" role="menuitem" aria-current="page"> 2. Trin 2 (valgt) </a>
                      </li>
                      <li role="none">
                        <a href="#" role="menuitem"> 3. Trin 3 </a>
                      </li>
                    </ul>
                  </nav>
                </div>
              </div>

              <div v-for="(step, stepIndex) of section.steps" :key="stepIndex">
                <fieldset v-if="stepIndex === currentStep" class="form-group">
                  <h3 class="h2 mt-0">{{ step.headline }}</h3>
                  <p>{{ step.description }}</p>
                  <div v-for="(question, questionIndex) of step.questions" :key="questionIndex">
                    <legend class="h5">{{ question.label }}</legend>
                    <ul class="nobullet-list" v-if="question.type === 'radio'">
                      <li v-for="(option, index) of question.options" :key="index">
                        <input
                          :id="`radio-${questionIndex}-${index}`"
                          type="radio"
                          :name="question.name"
                          :value="option.value ? option.value : index + 1"
                          class="form-radio"
                          :checked="values[question.name] === index + 1"
                          required
                          v-on="{ input, blur }"
                        />
                        <label :id="`form-label-radio-${questionIndex}-${index}`" :for="`radio-${questionIndex}-${index}`">{{ option.label }} </label>
                      </li>
                    </ul>
                    <div v-else class="form-range">
                      <input
                        :id="`range-${questionIndex}`"
                        type="range"
                        :name="question.name"
                        :value="values[question.name]"
                        :max="question.options.length"
                        min="1"
                        required
                        v-on="{ input, blur }"
                      />
                      <div class="sliderOptions">
                        <div
                          v-for="(option, optionIndex) in question.options"
                          :key="option.label"
                          :class="['sliderOptions_item', optionIndex + 1 == values[question.name] ? 'selected' : '']"
                        >
                          {{ option.label }}
                        </div>
                      </div>
                      <!-- <label :id="`form-label-range-${questionIndex}`" :for="`range-${questionIndex}`">{{ option.label }} </label> -->
                    </div>
                  </div>
                  <button
                    class="button button-primary d-block mt-7"
                    @click.prevent="currentStep + 1 === section.steps.length ? handleSubmit() : currentStep++"
                  >
                    {{ currentStep === 0 ? 'Start testen' : currentStep + 1 === section.steps.length ? 'Se resultat' : 'Næste' }}
                  </button>
                  <button v-if="currentStep > 0" class="back-link d-block mt-3" @click.prevent="currentStep--">Forrige</button>
                </fieldset>
              </div>
              <div v-if="currentStep === section.steps.length">
                <div class="row">
                  <div class="col">
                    <h2 class="h4">Pres for forreningsmodellen fordelt på områder</h2>
                    <apexchart
                      v-if="response"
                      height="500px"
                      type="radar"
                      :options="radarOptions"
                      :series="[
                        {
                          name: 'Din virksomhed',
                          data: response.filter(dataPoint => dataPoint.category).map(dataPoint => dataPoint.mean)
                        },
                        {
                          name: 'Branchen',
                          data: response.filter(dataPoint => dataPoint.category).map(dataPoint => dataPoint.mean_industry)
                        },
                        {
                          name: 'Alle virksomheder',
                          data: response.filter(dataPoint => dataPoint.category).map(dataPoint => dataPoint.mean_all)
                        }
                      ]"
                    ></apexchart>
                  </div>
                </div>
              </div>
            </template>
          </SimpleForm>
        </template>
      </div>
      <!-- test 1 end -->
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Watch, Vue } from 'vue-property-decorator';
import axios from 'axios';
import SimpleForm from 'vue-simpleform';
import VueApexCharts from 'vue-apexcharts';
import * as DKFDS from 'dkfds';

@Component({
  name: 'Applikation',
  components: {
    SimpleForm,
    apexchart: VueApexCharts
  }
})
export default class Applikation extends Vue {
  response = {} as any;
  private error = {};
  isLoading = false;
  currentStep = 0; // initial value 1
  currentSection = 'frontpage'; // initial value frontpage - possible values 'frontpage', 'test1', 'test2'

  apiBaseUrl = 'https://vg-api.irisgroup.dk/api/';
  defaultOptions = [
    { label: '1' },
    { label: '2' },
    { label: '3' },
    { label: '4' },
    { label: '5' },
    { label: '6' },
    { label: '7' },
    { label: '8' },
    { label: '9' },
    { label: '10' }
  ];
  industries = [
    { label: 'Industri', value: 'Industri' },
    { label: 'Bygge og anlæg', value: 'Bygge og anlæg' },
    { label: 'Handel', value: 'Handel' },
    { label: 'Transport', value: 'Transport' },
    { label: 'Videnservice', value: 'Videnservice' },
    { label: 'Kommunikation og reklame', value: 'Kommunikation og reklame' },
    { label: 'Operationel service', value: 'Operationel service' },
    { label: 'Oplevelseserhverv', value: 'Oplevelseserhverv' },
    { label: 'Andet (angiv hvilket)', value: 'Andet (angiv hvilket)' }
  ];
  defaultDescription = 'Angiv hvor enig du er i de enkelte udsagn, hvor 10= meget enig og 1= meget uenig.';
  sections = [
    {
      id: 'test1',
      headline: 'Hvordan er presset på din forretningsmodel?',
      description:
        'Svar på en række udsagn om din virksomhed inden for 6 områder og få en indikation af, hvordan presset er på din forretningsmodel.',
      steps: [
        {
          questions: [
            {
              name: 'industry',
              type: 'radio',
              label: 'Vælg din virksomheds branche og start testen',
              options: this.industries
            }
          ]
        },
        {
          headline: 'Interne mål',
          description: this.defaultDescription,
          questions: [
            {
              name: 'internal1',
              label: 'Vi har inden for de seneste 1-2 år ændret markant i de strategiske mål for virksomheden',
              options: this.defaultOptions
            },
            {
              name: 'internal2',
              label: 'Vi har opsat mål om at satse markant på nye markeder og forretningsområder',
              options: this.defaultOptions
            },
            {
              name: 'internal3',
              label: 'Vi har svært ved at realisere de opstillede mål i virksomheden',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Marked og efterspørgsel',
          description: this.defaultDescription,
          questions: [
            {
              name: 'market1',
              label: 'Vi oplever et stigende prispres i vores branche',
              options: this.defaultOptions
            },
            {
              name: 'market2',
              label: 'Vi oplever en stigende konkurrence på kvalitet og innovation i vores branche',
              options: this.defaultOptions
            },
            {
              name: 'market3',
              label: 'Branchen/markedet påvirkes af nye trends, der skaber forandringer i behov hos kunder og slutbrugere',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Teknologi',
          description: this.defaultDescription,
          questions: [
            {
              name: 'technology1',
              label: 'Ny teknologi rummer store potentialer for at forbedre/effektivisere produktion og processer i vores branche',
              options: this.defaultOptions
            },
            {
              name: 'technology2',
              label: 'Ny teknologi rummer store potentialer for at udvikle bedre løsninger/værditilbud i vores branche',
              options: this.defaultOptions
            },
            {
              name: 'technology3',
              label: 'Digitale teknologier rummer store potentialer for at styrke salg og demonstration af løsninger i vores branche',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Forsyningskæde',
          description: this.defaultDescription,
          questions: [
            {
              name: 'supplychain1',
              label: 'Strategier for valg af leverandører er under forandring i vores branche',
              options: this.defaultOptions
            },
            {
              name: 'supplychain2',
              label: 'Der sker en udvikling i retning af færre mellemled og mere direkte salg til slutbrugerne i vores branche',
              options: this.defaultOptions
            },
            {
              name: 'supplychain3',
              label: 'Håndtering af risici på forsyningssiden bliver en vigtigere konkurrencefaktor i vores branche',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Bæredygtighed og grøn omstilling',
          description: this.defaultDescription,
          questions: [
            {
              name: 'sustainability1',
              label: 'Vi oplever et stigende pres fra vores kunder/slutbrugere for bæredygtige løsninger og grøn omstilling ',
              options: this.defaultOptions
            },
            {
              name: 'sustainability2',
              label: 'Det bliver vigtigere at kunne dokumentere bæredygtighed og CO2-aftryk i vores branche',
              options: this.defaultOptions
            },
            {
              name: 'sustainability3',
              label: 'Den grønne omstilling skaber behov for partnerskaber og samarbejder i vores branche eller forsyningskæde',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Usikkerhed og kriser',
          description: this.defaultDescription,
          questions: [
            {
              name: 'unsecurity1',
              label: 'Vi oplever stigende usikkerhed om udviklingen i efterspørgsel og ordretilgang',
              options: this.defaultOptions
            },
            {
              name: 'unsecurity2',
              label: 'Vi oplever stigende risici i vores marked i relation til politiske forhold',
              options: this.defaultOptions
            },
            {
              name: 'unsecurity3',
              label: 'Vi har et behov for at skabe modstandskraft i forhold eventuelle nye kriser, pandemier, nedlukninger, handelskrige, mv.',
              options: this.defaultOptions
            }
          ]
        }
      ]
    },
    {
      id: 'test2',
      headline: 'Hvor kan din forretningsmodel forbedres?',
      description:
        'Svar på en række udsagn om din virksomhed inden for forretningsmodellens hovedområder – værditilbud, kunder, salg og kommunikation samt ressourcer, parnere og processer.',
      steps: [
        {
          questions: [
            {
              name: 'industry',
              label: 'Vælg din virksomheds branche og start testen',
              type: 'radio',
              options: this.industries
            }
          ]
        },
        {
          headline: 'Værditilbud',
          description: this.defaultDescription,
          questions: [
            {
              name: 'value1',
              label: 'Vi har behov for at blive klarere på, hvordan vi kan differentiere os fra vores konkurrenter',
              options: this.defaultOptions
            },
            {
              name: 'value2',
              label: 'Vi har et potentiale for at indtænke nye trends (fx bæredygtighed) i vores løsninger',
              options: this.defaultOptions
            },
            {
              name: 'value3',
              label: 'Vi har et potentiale for at udvikle nye betalingsmodeller for vores løsninger (fx abonnementer eller betaling efter forbrug) ',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Kunder',
          description: this.defaultDescription,
          questions: [
            {
              name: 'customers1',
              label: 'Vi mangler indsigt i kundernes beslutningsprocesser ved køb af vores løsninger',
              options: this.defaultOptions
            },
            {
              name: 'customers2',
              label: 'Vi har behov for at blive bedre til at inddele vores kunder i forskellige typer/segmenter',
              options: this.defaultOptions
            },
            {
              name: 'customers3',
              label: 'Vi har et potentiale for at udvikle tættere relationer til vigtige kunder',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Salg og kommunikation',
          description: this.defaultDescription,
          questions: [
            {
              name: 'sales1',
              label: 'Vi har behov for at udvikle nye tilgange ift. at formidle værdien af vores løsninger',
              options: this.defaultOptions
            },
            {
              name: 'sales2',
              label: 'Vi har behov for at udvikle flere eller mere velfungerende salgskanaler',
              options: this.defaultOptions
            },
            {
              name: 'sales3',
              label: 'Vi har udfordringer med at kommunikere vores værdier og profil til markedet',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Ressourcer, partnere og processer',
          description: this.defaultDescription,
          questions: [
            {
              name: 'resources1',
              label:
                'Vi har et potentiale for at øge produktiviteten i driften, fx gennem teknologi, ændret arbejdsorganisering eller styrket medarbejdermotivation',
              options: this.defaultOptions
            },
            {
              name: 'resources2',
              label: 'Vi oplever pres på vores kapacitet og interne logistik',
              options: this.defaultOptions
            },
            {
              name: 'resources3',
              label: 'Vi har behov for at styrke vores forretning på leverandørsiden og/eller optimere den samlede forsyningskæde',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Værditilbud/Kunder',
          description: this.defaultDescription,
          questions: [
            {
              name: 'valuecustomer1',
              label: 'Vi kan blive bedre til at målrette vores løsninger til specifikke kundegrupper',
              options: this.defaultOptions
            },
            {
              name: 'valuecustomer2',
              label: 'Vi har behov for bedre overblik over hvilke kunder, der køber hvilke produkter/løsninger',
              options: this.defaultOptions
            },
            {
              name: 'valuecustomer3',
              label: 'Vi har behov for at styrke vores evne til at prissætte vores løsninger over for forskellige kunder',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Kunder/Salg og kommunikation',
          description: this.defaultDescription,
          questions: [
            {
              name: 'customersales1',
              label: 'Vi  har behov for at tilpasse vores kommunikation og salgsindsats til forskellige kundetyper',
              options: this.defaultOptions
            },
            {
              name: 'customersales2',
              label: 'Vi mangler kanaler/værktøjer ift. at identificere og komme i dialog med potentielle kunder ',
              options: this.defaultOptions
            },
            {
              name: 'customersales3',
              label: 'Vi har behov for at skabe bedre indgange for kunderne, fx i forhold til at give os feedback og efterspørge opfølgende service',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Salg og kommunikation/Ressourcer',
          description: this.defaultDescription,
          questions: [
            {
              name: 'salesresources1',
              label: 'Vi har behov for nye kompetencer i forhold til at styrke demonstration og salg af vores løsninger',
              options: this.defaultOptions
            },
            {
              name: 'salesresources2',
              label: 'Vi har udfordringer med at strukturere og følge op på vores salgsarbejde',
              options: this.defaultOptions
            },
            {
              name: 'salesresources3',
              label: 'Vi har behov for nye kompetencer i forhold til internationalt salg og distribution',
              options: this.defaultOptions
            }
          ]
        },
        {
          headline: 'Ressourcer/værditilbud',
          description: this.defaultDescription,
          questions: [
            {
              name: 'resourcesvalue1',
              label: 'Vi har behov for nye kompetencer eller ny teknologi for at udvikle bedre løsninger til kunderne',
              options: this.defaultOptions
            },
            {
              name: 'resourcesvalue2',
              label: 'Vi har behov for at etablere nye partnerskaber for at udvikle bedre løsninger til kunderne',
              options: this.defaultOptions
            },
            {
              name: 'resourcesvalue3',
              label: 'Vi har for høje omkostninger i forhold til kvaliteten af vores løsninger/produkter',
              options: this.defaultOptions
            }
          ]
        }
      ]
    }
  ];

  values = [] as any;
  initialValues = {
    industry: 'Industri',
    internal1: 0,
    internal2: 0,
    internal3: 0,
    internal4: 0,
    market1: 0,
    market2: 0,
    market3: 0,
    market4: 0,
    technology1: 0,
    technology2: 0,
    technology3: 0,
    technology4: 0,
    supplychain1: 0,
    supplychain2: 0,
    supplychain3: 0,
    supplychain4: 0,
    sustainability1: 0,
    sustainability2: 0,
    sustainability3: 0,
    sustainability4: 0,
    unsecurity1: 0,
    unsecurity2: 0,
    unsecurity3: 0,
    unsecurity4: 0,
    value1: 0,
    value2: 0,
    value3: 0,
    value4: 0,
    customers1: 0,
    customers2: 0,
    customers3: 0,
    customers4: 0,
    sales1: 0,
    sales2: 0,
    sales3: 0,
    sales4: 0,
    resources1: 0,
    resources2: 0,
    resources3: 0,
    resources4: 0,
    valuecustomer1: 0,
    valuecustomer2: 0,
    valuecustomer3: 0,
    valuecustomer4: 0,
    customersales1: 0,
    customersales2: 0,
    customersales3: 0,
    customersales4: 0,
    salesresources1: 0,
    salesresources2: 0,
    salesresources3: 0,
    salesresources4: 0,
    resourcesvalue1: 0,
    resourcesvalue2: 0,
    resourcesvalue3: 0,
    resourcesvalue4: 0
  };

  get radarOptions() {
    if (!this.response) {
      return null;
    }

    const chartColors = {
      textColor: '#1A1A1A'
    };

    const categories = this.response.filter((dataPoint: any) => dataPoint.category).map((dataPoint: any) => dataPoint.category);
    return {
      chart: {
        id: 'radar',
        // foreColor: this.chartColors.textColor,
        // fontFamily: 'Helvetica Neue, Helvetica, sans-serif',
        fontFamily: 'IBMPlexSans, system',
        offsetY: -25,
        toolbar: {
          show: false
        }
      },
      stroke: {
        width: 1
        // colors: [this.chartColors.blueSolid, this.chartColors.orangeSolid, this.chartColors.greenSolid]
      },
      plotOptions: {
        radar: {
          size: 150,
          polygons: {
            strokeColor: '#e8e8e8',
            fill: {
              colors: ['#fff']
            }
          }
        }
      },
      xaxis: {
        categories: categories,
        labels: {
          style: {
            colors: Array(categories.length).fill(chartColors.textColor),
            fontSize: 13
          }
        }
      },
      yaxis: {
        min: 0,
        max: 10,
        tickAmount: 5,
        labels: {
          offsetX: 8,
          style: {
            colors: Array(categories.length).fill(chartColors.textColor),
            fontSize: 12
          }
        }
      },
      markers: {
        size: 4,
        hover: {
          size: 7
        }
      },
      legend: {
        fontSize: '14px',
        itemMargin: {
          horizontal: 8,
          vertical: 8
        },
        offsetY: -50
        // markers: {
        //   width: 20,
        //   height: 20,
        //   radius: 5,
        //   strokeWidth: 1,
        //   strokeColor: [this.chartColors.blueSolid, this.chartColors.orangeSolid, this.chartColors.greenSolid]
        // }
      }
    };
  }

  @Watch('currentStep')
  @Watch('currentSection')
  onStepChanged(value: string, oldValue: string) {
    // updated
    window.scrollTo(0, 0);
    this.validate(this.values);
    // this.maxStep = this.maxStep > this.currentStep ? this.maxStep : this.currentStep;
    // this.error = '';
    // this.errorHeading = '';
  }

  mounted() {
    DKFDS.init();
    // this.isLoading = true;
    // new DKFDS.Dropdown(document.getElementById('overflow-button'));
    // this.callExternalApi();
  }

  private async callExternalApi() {}

  handleSubmit({ values, errors, setSubmitting, setSubmitted }: any) {
    console.log('submit');
    console.log(values);
    console.log(this.values);
    this.isLoading = true;
    const answers = {};
    Object.entries(this.values)
      .filter(([key, value]) => key !== 'industry')
      .forEach(([key, value]) => {
        answers[key] = parseInt(value, 10);
      });
    console.log(answers);
    const data = JSON.stringify({
      answers: answers,
      industry: this.values.industry
    });
    console.log(data);
    axios
      .post(this.apiBaseUrl + '/feedback/' + this.currentSection.replace('test', ''), data, {
        headers: {
          'Content-Type': 'application/json'
        }
      })
      .then((rsp: any) => {
        console.log(rsp.data);
        this.currentStep++;
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

  validate(values: any) {
    this.values = values;
    console.log(values);
    return {
      email: 'Email is invalid'
    };
  }
}
</script>
<style lang="scss" scoped>
html {
  scroll-behavior: smooth;
}

.back-link {
  border: none;
  background: none;
}

.card {
  box-shadow: none;
  border-color: #d7dadf;
}

img {
  margin: 0 auto;
}

.flex-column {
  flex-direction: column;
  height: 100%;
  justify-content: space-between;

  .card {
    flex-grow: 1;
  }
}

.form-range {
  max-width: 66ch;
  margin-top: 1rem;
}

.sliderOptions {
  display: flex;
  justify-content: space-between;
  position: relative;

  &_item {
    display: flex;
    justify-content: center;
    text-align: center;
    width: 1px;
    white-space: nowrap;
    position: relative;
    font-size: 12px;
    // margin-top: 12px;

    // &:before {
    //   content: '';
    //   width: 1px;
    //   height: 24px;
    //   display: block;
    //   position: absolute;
    //   visibility: visible;
    //   z-index: 0;
    //   top: -28px;
    //   background: #747474;
    // }
  }
}
</style>
