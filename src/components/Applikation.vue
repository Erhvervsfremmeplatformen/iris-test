<template>
  <div class="applikation-container">
    <form @submit.prevent="handleSubmit">
      <div class="row">
        <div v-if="isLoading" class="spinner" aria-label="Henter indhold" />

        <!-- Frontpage start -->
        <template v-else-if="currentSection === 'frontpage'">
          <div class="col-12 align-text-center pt-7">
            <h1>Test og styrk din virksomheds forretningsmodel</h1>
            <div class="h2 m-4">med diagnoseværktøj i to dele</div>
            <p class="font-lead" style="max-width: none">Vælg en af af de to tests nedenfor</p>
            <div class="row" style="justify-content: center">
              <div class="m-3 m-md-2 px-4 px-md-0">
                <button class="button button-primary" @click.prevent="currentSection = 'test1'">Vurder presset på din forretningsmodel</button>
              </div>
              <div class="m-3 m-md-2 px-4 px-md-0">
                <button class="button button-primary" @click.prevent="currentSection = 'test2'">Få forbedringer til din forretningsmodel</button>
              </div>
            </div>
          </div>
          <div class="bg-normal full-width mt-9 p-md-7">
            <div class="container">
              <div class="row">
                <div class="col-md-6 mb-7">
                  <h2 class="mt-4">Hvorfor teste din forretningsmodel?</h2>
                  <p>
                    Der kan være mange årsager til, at en forretningsmodel kommer under pres, eller at der opstår nye muligheder i markedet, der gør
                    det relevant at styrke eller justere forretningsmodelen. Formålet med de to test ser at give dig en indikation af, om der kan være
                    behov for at efterse hele eller dele af din virksomheds forretningsmodel.
                  </p>
                  <h2>Hvem kan tage testen?</h2>
                  <p>
                    De to tests er målrettet små og mellemstore virksomheder. Den kan besvares af ledelsen eller medarbejdere, og kan med fordel
                    besvares af flere fra samme virksomhed, så svarene eventuelt kan sammenlignes internt.
                  </p>
                  <p>
                    Testene baserer sig på besvarelser fra en stor undersøgelse blandt mindre og mellemstore virksomheder, der succesfuldt har
                    forandret eller styrket deres forretningsmodel.
                  </p>
                </div>
                <div class="col-md-6 mb-7">
                  <div class="card card-border">
                    <div class="card-header px-md-7">
                      <h2>Sådan fungerer testen</h2>
                    </div>
                    <div class="card-text px-md-7">
                      <p>
                        Testen baserer sig på en større undersøgelse blandt små og mellemstore virksomheder, der succesfuldt har forandret eller
                        styrket hele eller dele af forretningen. Den viser, at drivkræfterne bag forretningsmodeludvikling typisk falder inden for
                        seks hovedområder:
                      </p>
                      <ul>
                        <li v-for="(theme, index) of sections[0].steps" :key="index">{{ theme.headline }}</li>
                      </ul>
                      <p>
                        Derfor beder vi dig i den første test om at stilling til en række udsagn knyttet til disse hovedområder. I den anden test
                        beder vi dig om at tage stilling til de fire dele i forretningsmodellen – 1) virksomhedens værditilbud, 2) kunder, 3) Salg 4)
                        og kommunikation, Ressourcer, partnere og processer, samt samspillet mellem dem
                      </p>
                    </div>
                  </div>
                </div>
                <div class="col-md-6 mb-7">
                  <div class="d-flex flex-column">
                    <div class="card card-align-height w-percent-100">
                      <figure class="figure p-5 bg-alternative" v-html="imgs[0]"></figure>
                      <div class="card-text card-border">
                        <button class="card-link h3" @click.prevent="currentSection = 'test1'">Vurder presset på din forretningsmodel</button>
                        <p>
                          Der er flere ting der spiller ind, når en forretningsmodel kommer under pres. Tag testen og få en indikation af, om der kan
                          være behov for at efterse hele eller dele af din forretningsmodel.
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="col-md-6 mb-7">
                  <div class="d-flex flex-column">
                    <div class="card card-align-height w-percent-100">
                      <figure class="figure p-5 bg-alternative" v-html="imgs[1]"></figure>
                      <div class="card-text card-border">
                        <button class="card-link h3" @click.prevent="currentSection = 'test2'">Juster din forretningsmodel</button>
                        <p>
                          For at udvikle en god forretningsmodel kræver det balance i modellens fire elementer, samt samspillet mellem dem. Tag testen
                          og få inspiration til hvordan din forretningsmodel kan justeres eller forbedres.
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </template>
        <!-- Frontpage end -->

        <!-- test 1 + 2 loop -->
        <div v-for="(section, sectionIndex) of sections" v-else :key="sectionIndex" class="col-lg-12">
          <template v-if="currentSection === section.id">
            <div class="row">
              <div class="col-lg-9 mb-0">
                <div v-if="Object.keys(errors).length > 0" role="alert" aria-atomic="true" class="alert alert-error mt-0 mb-8">
                  <div class="alert-body">
                    <p class="alert-heading">Svar på alle udsagn, for at gå videre</p>
                    <ul v-for="(error, errorIndex) of errors" :key="errorIndex" class="alert-text nobullet-list">
                      <li>
                        <a class="function-link" :href="error.errorAnchorHref">{{ error.errorSummary }}</a>
                      </li>
                    </ul>
                  </div>
                </div>
                <p class="h6">{{ currentStep === section.steps.length ? 'Resultat' : 'Test' }}</p>

                <h2 class="h1">{{ section.headline }}</h2>
                <p v-if="(section.description || section.descriptionAlternative) && currentStep === 0" class="font-lead">
                  {{ currentSection === 'test2' && values.industry !== '' ? section.descriptionAlternative : section.description }}
                </p>
                <p v-if="(section.description || section.resultIntro) && currentStep === section.steps.length" class="font-lead">
                  {{ section.resultIntro }}
                </p>
              </div>
              <div class="col-lg-7 mb-7">
                <button
                  v-if="currentStep === section.steps.length && currentSection === 'test1'"
                  class="button button-primary"
                  @click.prevent="
                    currentSection = 'test2';
                    currentStep = 0;
                    skipIndustrySelect = true;
                  "
                >
                  Tag testen: Få forbedringer til din forretningsmodel
                </button>

                <div v-if="currentStep > 0 && currentStep < section.steps.length" class="overflow-menu overflow-menu--open-right">
                  <button
                    id="overflow-button"
                    class="button-overflow-menu js-dropdown"
                    data-js-target="overflow5"
                    aria-haspopup="true"
                    aria-expanded="false"
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
                  <fieldset v-if="stepIndex === currentStep">
                    <h3 v-if="step.headline" class="h2 mt-0">{{ step.headline }}</h3>
                    <p>{{ step.description }}</p>
                    <div
                      v-for="(question, questionIndex) of step.questions"
                      :id="`form-group-${stepIndex}-${questionIndex}`"
                      :key="questionIndex"
                      :class="['form-group', 'mb-7', errors[question.name] ? 'form-error' : '']"
                    >
                      <div v-if="currentStep > 0 || currentSection === 'test1' || (currentStep === 0 && !skipIndustrySelect)">
                        <legend :id="`label-${stepIndex}-${questionIndex}`" class="h5 mb-4">{{ question.label }}</legend>
                        <span v-if="errors[question.name]" class="form-error-message">
                          <span class="sr-only">Fejl:</span> {{ errors[question.name].errorMessage }}
                        </span>
                        <ul v-if="question.type === 'radio'" class="nobullet-list">
                          <li v-for="(option, index) of question.options" :key="index">
                            <input
                              :id="`radio-${questionIndex}-${index}`"
                              type="radio"
                              :autofocus="questionIndex === 0"
                              :name="question.name"
                              :value="option.value"
                              class="form-radio radio-large"
                              :checked="values[question.name] === option.value"
                              required
                              @input="updateValue(question.name, $event.target.value)"
                            />
                            <label :id="`form-label-radio-${questionIndex}-${index}`" :for="`radio-${questionIndex}-${index}`"
                              >{{ option.label }}
                            </label>
                          </li>
                        </ul>
                        <div v-else class="form-range">
                          <input
                            :id="`range-${questionIndex}`"
                            type="range"
                            :max="question.options.length - 1"
                            :autofocus="questionIndex === 0"
                            min="0"
                            required
                            :aria-valuemax="question.options.length - 1"
                            aria-valuemin="0"
                            :value="values[question.name]"
                            :name="question.name"
                            :aria-labelledby="`label-${stepIndex}-${questionIndex}`"
                            aria-role="slider"
                            :aria-valuenow="values[question.name]"
                            :class="`slider-${values[question.name]}`"
                            @input="updateValue(question.name, parseInt($event.target.value))"
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
                    </div>
                    <button
                      id="primaryButton"
                      class="button button-primary d-block mt-7"
                      @click.prevent="handleSubmit()"
                      v-if="currentStep + 1 === section.steps.length"
                    >
                      Se resultat
                    </button>
                    <button id="primaryButton" class="button button-primary d-block mt-7" @click.prevent="goToNextStep()" v-else>
                      {{ currentStep > 0 ? 'Næste' : skipIndustrySelect ? 'Fortsæt' : 'Start testen' }}
                    </button>
                    <button class="back-link d-block mt-3" @click.prevent="currentStep > 0 ? currentStep-- : (currentSection = 'frontpage')">
                      {{ currentStep > 0 ? 'Forrige' : 'Tilbage' }}
                    </button>
                  </fieldset>
                </div>
              </div>
              <div v-if="currentStep === 0" :class="currentSection === 'test1' ? 'col-lg-5' : 'col-lg-4'">
                <figure class="figure" v-html="currentSection === 'test1' ? imgs[0] : imgs[1]"></figure>
              </div>

              <!-- Results start -->
              <div v-if="currentStep === section.steps.length" class="col-12">
                <div class="bg-normal full-width mt-3 p-md-7">
                  <div class="container">
                    <div class="row">
                      <div class="col-12">
                        <h2>Dit resultat</h2>
                        <button class="button button-secondary">
                          <svg class="icon-svg" focusable="false" aria-hidden="true">
                            <use xlink:href="#download"></use></svg
                          >Download resultatet som PDF
                        </button>
                      </div>
                      <div class="col-xl-7">
                        <h3 class="h4">{{ section.resultPrimaryHeadline }}</h3>
                        <apexchart
                          v-if="response[currentSection]"
                          type="radar"
                          :options="radarOptions"
                          height="400"
                          :series="radarData()"
                        ></apexchart>
                      </div>
                      <div class="col-xl-5">
                        <h3 class="h4">{{ section.resultSecondaryHeadline }}</h3>
                        <apexchart
                          v-if="response[currentSection] && currentSection === 'test1'"
                          type="bar"
                          height="300"
                          :options="columnOptions()"
                          :series="[
                            {
                              name: 'Presset på forretningsmodellen',
                              data: columnData()
                            }
                          ]"
                        ></apexchart>
                        <div v-else>
                          <p v-for="link of response[currentSection].suggestions.slice(0, 4)" :key="link.theme">
                            <a :href="link.theme"
                              >{{ link.description }}
                              <svg class="icon-svg" focusable="false" aria-hidden="true">
                                <use xlink:href="#open-in-new"></use>
                              </svg>
                            </a>
                          </p>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <div v-if="currentSection === 'test1'" class="row">
                  <div class="col-md-6 mt-7">
                    <div class="card card-blue">
                      <div class="card-header pt-md-7 px-md-7">
                        <h2>Behov for at justere i din forretningsmodel?</h2>
                      </div>
                      <div class="card-text px-md-7">
                        <p>Få inspiration til hvordan du kan styrke eller justere din forretningsmodel.</p>
                      </div>
                      <div class="card-footer pb-4 px-4 pb-md-7 px-md-7">
                        <button
                          v-if="currentStep === section.steps.length && currentSection === 'test1'"
                          class="button button-primary"
                          @click.prevent="
                            currentSection = 'test2';
                            currentStep = 0;
                            skipIndustrySelect = true;
                          "
                        >
                          Tag testen: Få forbedringer til din forretningsmodel
                        </button>
                      </div>
                    </div>
                  </div>
                </div>

                <button v-if="currentStep > 0" class="back-link d-block mt-5" @click.prevent="currentStep = 0">Tag testen igen</button>
              </div>
            </div>
          </template>
        </div>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { Component, Watch, Vue } from 'vue-property-decorator';
import axios from 'axios';
import VueApexCharts from 'vue-apexcharts';
import * as DKFDS from 'dkfds';

@Component({
  name: 'Applikation',
  components: {
    apexchart: VueApexCharts
  }
})
export default class Applikation extends Vue {
  response = {} as any;
  private error = {};
  errors = {} as any;
  isLoading = false;
  currentStep = 0; // initial value 0
  currentSection = 'frontpage'; // initial value frontpage - possible values 'frontpage', 'test1', 'test2'
  imgs = [] as any;
  sessionId = this.generateId(32);
  skipIndustrySelect = false;

  apiBaseUrl = 'https://vg-api.irisgroup.dk/api/';
  defaultOptions = [
    { label: '0' },
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
    { label: 'Andet', value: 'Andet' }
  ];
  defaultDescription = 'Angiv hvor enig du er i de enkelte udsagn, hvor 10= meget enig og 1= meget uenig.';
  sections = [
    {
      id: 'test1',
      headline: 'Hvordan er presset på din forretningsmodel?',
      resultIntro:
        'Nedenfor finder du et overblik over hvordan presset på din virksomhed er, sammen med et gennemsnit af hvordan andre virksomheder inden for sammen branche klarer sig. Vi anbefaler at du også tager testens anden del, der viser dig, hvor din forretningsmodel potentielt kan styrkes. ',
      resultPrimaryHeadline: 'Pres for forretningsmodellen fordelt på områder',
      description:
        'Svar på en række udsagn om din virksomhed inden for 6 områder og få en indikation af, hvordan presset er på din forretningsmodel.',
      resultSecondaryHeadline: 'Samlet billede af presset på forretningsmodellen',
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
      descriptionAlternative:
        'Svar på en række udsagn om din virksomhed inden for forretningsmodellens hovedområder – værditilbud, kunder, salg og kommunikation samt ressourcer, parnere og processer. Til sidst får du et samlet resultat, der både viser dig hvordan presset er på din virksomhed, samt giver dig inspiration til, hvordan din forretningsmodel kan styrkes.',
      resultIntro:
        'Nedenfor finder du et overblik over hvor der kan være potentiale for at styrke din forretningsmodel sammenlignet med branchen. Desuden finder du inspiration til hvordan din forretningsmodel potentielt kan styrkes, baseret på dine svar. ',
      resultPrimaryHeadline: 'Din forretningsmodel kan styrkes på følgende områder ',
      resultSecondaryHeadline: 'Ud fra dine svar ser der især ud til at være potentiale for at styrke din forretningsmodel på følgende områder',
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
  ] as any;

  // values = [] as any;
  values = {
    industry: '',
    internal1: 0,
    internal2: 0,
    internal3: 0,
    market1: 0,
    market2: 0,
    market3: 0,
    market4: 0,
    technology1: 0,
    technology2: 0,
    technology3: 0,
    supplychain1: 0,
    supplychain2: 0,
    supplychain3: 0,
    sustainability1: 0,
    sustainability2: 0,
    sustainability3: 0,
    unsecurity1: 0,
    unsecurity2: 0,
    unsecurity3: 0,
    value1: 0,
    value2: 0,
    value3: 0,
    customers1: 0,
    customers2: 0,
    customers3: 0,
    sales1: 0,
    sales2: 0,
    sales3: 0,
    resources1: 0,
    resources2: 0,
    resources3: 0,
    valuecustomer1: 0,
    valuecustomer2: 0,
    valuecustomer3: 0,
    customersales1: 0,
    customersales2: 0,
    customersales3: 0,
    salesresources1: 0,
    salesresources2: 0,
    salesresources3: 0,
    resourcesvalue1: 0,
    resourcesvalue2: 0,
    resourcesvalue3: 0
  } as any;

  columnData() {
    if (this.response[this.currentSection].scores[0].mean_score_industry) {
      return [
        parseInt(this.response[this.currentSection].scores[0].score_own, 10),
        parseInt(this.response[this.currentSection].scores[0].mean_score_industry, 10),
        parseInt(this.response[this.currentSection].scores[0].mean_score_all, 10)
      ];
    }
    return [
      parseInt(this.response[this.currentSection].scores[0].score_own, 10),
      parseInt(this.response[this.currentSection].scores[0].mean_score_all, 10)
    ];
  }

  radarData() {
    const categoryMeans = this.response[this.currentSection].category_means.filter((dataPoint: any) => dataPoint.category);
    if (this.response[this.currentSection].category_means[0].mean_industry) {
      return [
        {
          name: 'Din virksomhed',
          data: categoryMeans.map((dataPoint: any) => dataPoint.mean_answers)
        },
        {
          name: 'Branchen',
          data: categoryMeans.map((dataPoint: any) => dataPoint.mean_industry)
        },
        {
          name: 'Alle virksomheder',
          data: categoryMeans.map((dataPoint: any) => dataPoint.mean_all)
        }
      ];
    }
    return [
      {
        name: 'Din virksomhed',
        data: categoryMeans.map((dataPoint: any) => dataPoint.mean_answers)
      },
      {
        name: 'Alle virksomheder',
        data: categoryMeans.map((dataPoint: any) => dataPoint.mean_all)
      }
    ];
  }

  get radarOptions() {
    if (!this.response) {
      return null;
    }

    const chartColors = {
      textColor: '#1A1A1A'
    };

    const categories = this.response[this.currentSection].category_means
      .filter((dataPoint: any) => dataPoint.category)
      .map((dataPoint: any) => dataPoint.category);
    return {
      chart: {
        id: 'radar',
        fontFamily: 'IBMPlexSans, system',
        toolbar: {
          show: false
        }
      },
      stroke: {
        width: 1
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
      responsive: [{ breakpoint: 576, options: { plotOptions: { radar: { size: 150 } }, xaxis: { labels: { show: false } } } }],
      xaxis: {
        categories: categories,
        labels: {
          show: true,
          style: {
            colors: Array(categories.length).fill(chartColors.textColor),
            fontSize: 12
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
        }
      }
    };
  }

  columnOptions() {
    if (!this.response) {
      return null;
    }

    const chartColors = {
      textColor: '#1A1A1A'
    };

    const categories = this.response[this.currentSection].scores[0].mean_score_industry
      ? ['Din virksomhed', 'Branchen', 'Alle virksomheder']
      : ['Din virksomhed', 'Alle virksomheder'];

    return {
      chart: {
        id: 'bar',
        fontFamily: 'IBMPlexSans, system',
        toolbar: {
          show: false
        }
      },
      plotOptions: {
        bar: {
          horizontal: false,
          columnWidth: '55%',
          distributed: true,
          endingShape: 'rounded'
        }
      },
      dataLabels: {
        enabled: false
      },
      xaxis: {
        categories: categories,
        labels: {
          style: {
            fontSize: 13
          }
        }
      },
      yaxis: {
        min: 0,
        max: 100,
        tickAmount: 5,
        labels: {
          offsetX: 8,
          style: {
            fontSize: 12
          }
        }
      },
      legend: {
        show: false
      }
    };
  }

  @Watch('currentStep')
  @Watch('currentSection')
  @Watch('errors')
  onStepChanged(value: string, oldValue: string) {
    window.scrollTo(0, 0);
    // DKFDS.init();
  }

  generateId(length: number) {
    let result = '';
    const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    const charactersLength = characters.length;
    for (let i = 0; i < length; i++) {
      result += characters.charAt(Math.floor(Math.random() * charactersLength));
    }

    return result;
  }

  mounted() {
    // this.isLoading = true;
    // new DKFDS.Dropdown(document.getElementById('overflow-button'));
    DKFDS.init();
    this.getImages();
  }

  updated() {
    if (this.currentSection !== 'frontpage') {
      document.addEventListener('keydown', event => {
        if (event.key === 'Enter') {
          event.stopImmediatePropagation();
          document.getElementById('primaryButton')?.click();
        }
      });
    }
  }

  private async getImages() {
    this.isLoading = true;

    Promise.all([axios.get(this.apiBaseUrl + '/img/part1'), axios.get(this.apiBaseUrl + '/img/part2')]).then(responses => {
      const [url1rest, url2resp] = responses;
      this.imgs = [url1rest.data, url2resp.data];
      this.isLoading = false;
    });
  }

  updateValue(key: string, value: any) {
    this.$emit('input', (this.values[key] = value));
  }

  goToNextStep() {
    if (this.validate()) {
      this.currentStep++;
    }
  }

  handleSubmit() {
    if (!this.validate()) {
      return;
    }
    this.isLoading = true;
    const answers = {} as any;
    Object.entries(this.values)
      .filter(([key, value]) => key !== 'industry')
      .forEach(([key, value]) => {
        answers[key] = value;
      });
    console.log(answers);
    const data = JSON.stringify({
      answers: answers,
      industry: this.values.industry,
      sessionID: this.sessionId,
      time: Date()
    });

    const requests = [
      axios.post(this.apiBaseUrl + '/feedback/' + this.currentSection.replace('test', ''), data, {
        headers: {
          'Content-Type': 'application/json'
        }
      })
    ];
    if (this.currentSection === 'test2') {
      requests.push(
        axios.post(this.apiBaseUrl + '/suggestions', data, {
          headers: {
            'Content-Type': 'application/json'
          }
        })
      );
    }
    Promise.all([...requests])
      .then((rsp: any) => {
        this.currentStep++;
        this.isLoading = false;
        if (!rsp[0].data.error) {
          this.isLoading = false;
          if (rsp.length > 1) {
            this.response[this.currentSection] = { ...rsp[0].data, suggestions: rsp[1].data };
          } else {
            this.response[this.currentSection] = rsp[0].data;
          }
          console.log(this.response[this.currentSection]);
          // this.response = rsp.data;
        }
      })
      .catch((error: any) => {
        console.log(error);
        this.isLoading = false;
        this.error = 'Noget gik galt. Prøv venligst igen.';
      });
  }

  validate() {
    this.errors = {};

    this.sections
      .find((section: any) => section.id === this.currentSection)
      .steps[this.currentStep].questions.forEach((question: any, index: any) => {
        if (this.values[question.name] === 0) {
          this.errors[question.name] = {
            errorSummary: 'Angiv hvor enig du er i dette udsagn på en skala fra 1-10',
            errorMessage: 'Angiv hvor enig du er i dette udsagn. Vælg 1, hvis spørsmålet ikke er relevant for din virksomhed. ',
            errorAnchorHref: `#form-group-${this.currentStep}-${index}`
          };
        }
        if (this.values[question.name] === '') {
          this.errors[question.name] = {
            errorSummary: 'Vælg din virksomheds branche',
            errorMessage: 'Vælg din virksomheds branche for at starte testen',
            errorAnchorHref: `#form-group-${this.currentStep}-${index}`
          };
        }
      });

    return Object.keys(this.errors).length === 0;
    // return true;
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
  border-color: transparent;

  &-blue {
    border: none;

    & >>> *:not(.button) {
      background-color: #e7efff;
    }
  }

  &-border {
    border: 1px solid #d7dadf;
  }

  &-link {
    appearance: none;
    border: none;
    background: transparent;
    font-family: inherit;
    padding: 0;
    text-align: left;

    &:after {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
    }
  }
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

  input[type='range'] {
    padding: 12px 0;
    background: transparent;
    height: 8px;

    &::-moz-range-track {
      border: none;
      height: 8px;
      border-radius: 8px;
    }
    &::-webkit-slider-runnable-track {
      height: 8px;
      border: none;
      border-radius: 8px;
    }
    &::-webkit-slider-thumb {
      border: 1px solid #dcdcdc;
      box-shadow: 0px 2px 5px rgba(60, 66, 87, 0.08), 0px 1px 1px rgba(0, 0, 0, 0.12);
    }
    &::-moz-range-thumb {
      border: 1px solid #dcdcdc;
      box-shadow: 0px 2px 5px rgba(60, 66, 87, 0.08), 0px 1px 1px rgba(0, 0, 0, 0.12);
    }

    &:focus {
      // outline: none;
    }
  }

  .slider-0 {
    &::-moz-range-track {
      background: #bfbfbf;
    }
    &::-webkit-slider-runnable-track {
      background: #bfbfbf;
    }
  }

  @for $i from 1 through 10 {
    .slider-#{$i} {
      &::-moz-range-track {
        background: linear-gradient(to right, #0052ff 0%, #0052ff $i * 10%, #bfbfbf $i * 10%);
      }
      &::-webkit-slider-runnable-track {
        background: linear-gradient(to right, #0052ff 0%, #0052ff $i * 10%, #bfbfbf $i * 10%);
      }
    }
  }
}

.sliderOptions {
  display: flex;
  justify-content: space-between;
  position: relative;
  margin: 20px 12px 0;

  &_item {
    display: flex;
    justify-content: center;
    text-align: center;
    width: 1px;
    white-space: nowrap;
    position: relative;
    font-size: 13px;
    color: #454545;

    &:nth-of-type(1) {
      visibility: hidden;
      @include media-breakpoint-up(sm) {
        &:before {
          content: 'Meget uenig';
          visibility: visible;
          font-weight: bold;
          white-space: normal;
          margin-left: 24px;
          margin-top: -18px;
        }
      }
    }

    @include media-breakpoint-up(sm) {
      &:nth-of-type(11) {
        &:after {
          position: absolute;
          content: 'Meget enig';
          visibility: visible;
          font-weight: bold;
          white-space: normal;
          // margin-left: 24px;
          transform: translateX(calc(100% + 24px));
          text-align: right;
          margin-top: -18px;
        }
      }
    }
    // margin-top: 12px;

    &:not(:nth-of-type(1)):before {
      content: '';
      width: 1px;
      height: 8px;
      display: block;
      position: absolute;
      visibility: visible;
      z-index: 0;
      top: -16px;
      background: #999999;
    }
  }
}

.full-width {
  width: 100vw;
  margin-left: calc(50% - 50vw);
  margin-right: calc(50% - 50vw);
}

.icon-svg {
  height: 1.6rem;
  width: 1.6rem;
}

.figure {
  margin: 0;
  & >>> svg {
    width: auto;
    max-height: 400px;
    max-width: 100%;
    height: auto;
    margin: 0 auto;
    display: block;
  }
}
</style>
