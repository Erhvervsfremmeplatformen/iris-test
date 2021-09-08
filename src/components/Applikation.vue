<template>
  <div class="applikation-container">
    <div v-if="isLoading" class="spinner" aria-label="Henter indhold" />
    <div v-else class="row">
      <div class="col-lg-9">
        <!-- Frontpage start -->
        <template v-if="currentSection === 'frontpage'">
          <p class="h6">Digitalt værktøj</p>
          <h1>Test og styrk din virksomheds forretningsmodel</h1>
          <p class="font-lead">
            Et overblik over din virksomheds forretningsmodel, kan hjælpe dig med at få en indikation af om der er et behov for at justere eller
            efterse hele eller dele af forretningsmodellen.
          </p>
          <p>
            <button class="button button-primary" @click.prevent="currentSection = 'test1'">
              Tag testen: Vurder presset på din forretningsmodel
            </button>
          </p>
          <p>
            <button class="button button-primary" @click.prevent="currentSection = 'test2'">
              Tag testen: Få forbedringer til din forretningsmodel
            </button>
          </p>
        </template>
        <!-- Frontpage end -->

        <!-- test 1 + 2 loop -->
        <div v-for="(section, sectionIndex) of sections" :key="sectionIndex">
          <template v-if="currentSection === section.id">
            <p class="h6">Test</p>

            <h2 class="h1">{{ section.headline }}</h2>
            <p class="font-lead">
              {{ section.description }}
            </p>
            <div v-for="(step, stepIndex) of section.steps" :key="stepIndex" class="form-group">
              <fieldset v-if="stepIndex === currentStep">
                <h3 class="h2">{{ step.headline }}</h3>
                <p>{{ step.description }}</p>
                <div v-for="(question, questionIndex) of step.questions" :key="questionIndex">
                  <legend class="h5">{{ question.label }}</legend>
                  <ul class="nobullet-list">
                    <li v-for="(option, index) of question.options" :key="index">
                      <input :id="`radio-${questionIndex}-${index}`" type="radio" :name="questionIndex" :value="option" class="form-radio" />
                      <label :id="`form-label-radio-${questionIndex}-${index}`" :for="`radio-${questionIndex}-${index}`">{{ option }} </label>
                    </li>
                  </ul>
                </div>
                <p>
                  <button class="button button-primary" @click.prevent="currentStep++">{{ currentStep === 0 ? 'Start testen' : 'Næste' }}</button>
                </p>
                <p>
                  <button v-if="currentStep > 0" class="back-link" @click.prevent="currentStep--">Forrige</button>
                </p>
              </fieldset>
            </div>
          </template>
        </div>
        <!-- test 1 end -->
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Watch, Vue } from 'vue-property-decorator';
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
  currentStep = 0;
  currentSection = 'frontpage'; // initial value frontpage - possible values
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

  apiBaseUrl = 'https://vg-api.irisgroup.dk/api/';
  defaultOptions = ['1 = meget enig', '2', '3', '4', '5', '6', '7', '8', '9', '10 = meget enig'];
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
              label: 'Vælg din virksomheds branche og start testen',
              options: [
                'Industri',
                'Bygge og anlæg',
                'Handel',
                'Transport',
                'Videnservice',
                'Kommunikation og reklame',
                'Operationel service',
                'Oplevelseserhverv',
                'Andet (angiv hvilket)'
              ]
            }
          ]
        },
        {
          headline: 'Interne mål',
          description: this.defaultDescription,
          questions: [
            {
              label: 'Vi har inden for de seneste 1-2 år ændret markant i de strategiske mål for virksomheden',
              options: this.defaultOptions
            },
            {
              label: 'Vi har opsat mål om at satse markant på nye markeder og forretningsområder',
              options: this.defaultOptions
            },
            {
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
              label: 'Vi oplever et stigende prispres i vores branche',
              options: this.defaultOptions
            },
            {
              label: 'Vi oplever en stigende konkurrence på kvalitet og innovation i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Ny teknologi rummer store potentialer for at forbedre/effektivisere produktion og processer i vores branche',
              options: this.defaultOptions
            },
            {
              label: 'Ny teknologi rummer store potentialer for at udvikle bedre løsninger/værditilbud i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Strategier for valg af leverandører er under forandring i vores branche',
              options: this.defaultOptions
            },
            {
              label: 'Der sker en udvikling i retning af færre mellemled og mere direkte salg til slutbrugerne i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Vi oplever et stigende pres fra vores kunder/slutbrugere for bæredygtige løsninger og grøn omstilling ',
              options: this.defaultOptions
            },
            {
              label: 'Det bliver vigtigere at kunne dokumentere bæredygtighed og CO2-aftryk i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Vi oplever stigende usikkerhed om udviklingen i efterspørgsel og ordretilgang',
              options: this.defaultOptions
            },
            {
              label: 'Vi oplever stigende risici i vores marked i relation til politiske forhold',
              options: this.defaultOptions
            },
            {
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
              label: 'Vælg din virksomheds branche og start testen',
              options: [
                'Industri',
                'Bygge og anlæg',
                'Handel',
                'Transport',
                'Videnservice',
                'Kommunikation og reklame',
                'Operationel service',
                'Oplevelseserhverv',
                'Andet (angiv hvilket)'
              ]
            }
          ]
        },
        {
          headline: 'Interne mål',
          description: this.defaultDescription,
          questions: [
            {
              label: 'Vi har inden for de seneste 1-2 år ændret markant i de strategiske mål for virksomheden',
              options: this.defaultOptions
            },
            {
              label: 'Vi har opsat mål om at satse markant på nye markeder og forretningsområder',
              options: this.defaultOptions
            },
            {
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
              label: 'Vi oplever et stigende prispres i vores branche',
              options: this.defaultOptions
            },
            {
              label: 'Vi oplever en stigende konkurrence på kvalitet og innovation i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Ny teknologi rummer store potentialer for at forbedre/effektivisere produktion og processer i vores branche',
              options: this.defaultOptions
            },
            {
              label: 'Ny teknologi rummer store potentialer for at udvikle bedre løsninger/værditilbud i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Strategier for valg af leverandører er under forandring i vores branche',
              options: this.defaultOptions
            },
            {
              label: 'Der sker en udvikling i retning af færre mellemled og mere direkte salg til slutbrugerne i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Vi oplever et stigende pres fra vores kunder/slutbrugere for bæredygtige løsninger og grøn omstilling ',
              options: this.defaultOptions
            },
            {
              label: 'Det bliver vigtigere at kunne dokumentere bæredygtighed og CO2-aftryk i vores branche',
              options: this.defaultOptions
            },
            {
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
              label: 'Vi oplever stigende usikkerhed om udviklingen i efterspørgsel og ordretilgang',
              options: this.defaultOptions
            },
            {
              label: 'Vi oplever stigende risici i vores marked i relation til politiske forhold',
              options: this.defaultOptions
            },
            {
              label: 'Vi har et behov for at skabe modstandskraft i forhold eventuelle nye kriser, pandemier, nedlukninger, handelskrige, mv.',
              options: this.defaultOptions
            }
          ]
        }
      ]
    }
  ];
  steps = [];

  @Watch('currentStep')
  @Watch('currentSection')
  onStepChanged(value: string, oldValue: string) {
    // updated
    window.scrollTo(0, 0);
    // this.maxStep = this.maxStep > this.currentStep ? this.maxStep : this.currentStep;
    // this.error = '';
    // this.errorHeading = '';
  }

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
      .post(this.apiBaseUrl + '/suggestions', data, {
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
<style lang="scss" scoped>
html {
  scroll-behavior: smooth;
}

.back-link {
  border: none;
  background: none;
}
</style>
