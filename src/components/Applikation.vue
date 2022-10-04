<!-- Indgangspunktet for sandkasse-applikationen. Direkte og indirekte importering af komponenter og stylesheets i denne klasse vil blive inkluderet i den endelig applikation. -->
<template>
  <div class="applikation-container">
    <SvgIcons />
    <div v-if="Object.keys(errors).length > 0" role="alert" aria-atomic="true" class="alert alert-error mt-0 mb-8">
      <div class="alert-body">
        <p class="alert-heading">Svar på alle udsagn, for at gå videre</p>
        <ul v-for="(errorItem, errorIndex) of errors" :key="errorIndex" class="alert-text nobullet-list">
          <li>
            <a class="function-link" :href="errorItem.errorAnchorHref">{{ errorItem.errorSummary }}</a>
          </li>
        </ul>
      </div>
    </div>

    <div
      v-if="!isLoading && currentStep > 1 && currentStep < sections[currentSection.replace('test', '') - 1].steps.length"
      class="overflow-menu overflow-menu--open-right px-lg-7 px-xl-0"
    >
      <button id="overflow-button" class="button-overflow-menu js-dropdown" data-js-target="overflow5" aria-haspopup="true" aria-expanded="false">
        Trin {{ currentStep - 1 }} af {{ sections[currentSection.replace('test', '') - 1].steps.length - 2 }}
        <svg class="icon-svg" aria-hidden="true" focusable="false"><use xlink:href="#arrow-drop-down"></use></svg>
      </button>
      <div id="overflow5" class="overflow-menu-inner" aria-hidden="true">
        <nav>
          <ul class="overflow-list sidenav-list" role="menu">
            <li
              v-for="(step, stepIndex) of sections[currentSection.replace('test', '') - 1].steps.slice(2)"
              :key="`nav_step_${stepIndex}`"
              role="none"
              :class="[stepIndex + 2 === currentStep ? 'active current' : '', stepIndex > maxStep ? 'disabled' : '']"
            >
              <a
                href="#"
                role="menuitem"
                :aria-disabled="stepIndex > maxStep ? true : false"
                @click.prevent="stepIndex <= maxStep ? goToStep(stepIndex + 2) : ''"
              >
                {{ stepIndex + 1 }}. {{ step.headline }}
                <span v-if="stepIndex + 1 < maxStep" class="sidenav-icon">
                  <svg class="icon-svg" aria-hidden="true" focusable="false" tabindex="-1"><use xlink:href="#done"></use></svg>
                </span>
              </a>
            </li>
          </ul>
        </nav>
      </div>
    </div>
    <form @submit.prevent="handleSubmit">
      <div class="row">
        <div v-if="isLoading" class="spinner" aria-label="Henter indhold" />

        <!-- Frontpage start -->
        <template v-else-if="currentSection === 'frontpage'">
          <div class="col-12 align-text-center pt-7">
            <h1>Test og styrk virksomhedens forretningsmodel</h1>
            <div class="h2 m-4">med diagnoseværktøj i to dele</div>
            <figure class="figure alternative mb-6">
              <Logo style="max-width: 200px" />
            </figure>
            <p class="font-lead" style="max-width: none">
              Testen består af to dele.<br />
              Del 1 handler om presset på jeres nuværende forretningsmodel. <br />Del 2 handler om at identificere de områder, hvor jeres
              forretningsmodel kan styrkes.
            </p>
            <div class="row" style="justify-content: center">
              <div class="m-3 m-md-2 px-4 px-md-0">
                <button class="button button-primary" @click.prevent="currentSection = 'test1'">Start med del 1</button>
              </div>
              <div class="m-3 m-md-2 px-4 px-md-0">
                <button class="button button-primary" @click.prevent="currentSection = 'test2'">Gå direkte til del 2</button>
              </div>
            </div>
          </div>
          <div class="bg-normal full-width mt-9 p-lg-7">
            <div class="container">
              <div class="row">
                <div class="col-md-6 mb-7">
                  <h2 class="mt-4">Hvorfor teste jeres forretningsmodel?</h2>
                  <p>
                    Der kan være mange årsager til, at en forretningsmodel kommer under pres. Der kan også opstå nye muligheder i markedet, der gør
                    det relevant at styrke forretningsmodellen. Formålet med testen er at give jer en indikation af, om der kan være behov for at
                    efterse hele eller dele af forretningsmodellen.
                  </p>
                  <h2>Hvem kan tage testen?</h2>
                  <p>
                    Testen henvender sig primært til ledelsen i mindre virksomheder (fra ca. 5 til 100 ansatte). Den kan eventuelt besvares
                    individuelt af flere ledere fra samme virksomhed, så svarene kan sammenlignes og drøftes internt. I kan også vælge at udfylde den
                    sammen i ledelsen.
                  </p>
                  <p>
                    Testen er baseret på en stor analyse af erfaringer fra ca. 200 små og mellemstore virksomheder, der succesfuldt har omstillet
                    forretningsmodellen.
                    <a data-module="modal" href="#" data-target="modal-method" @click.prevent>Læs mere her</a>. Ved at gennemføre testen kommer I
                    dermed til at forholde jer til de udfordringer, andre virksomheder har stået over for på deres vej til bedre forretning.
                  </p>
                  <p>
                    Testen er udviklet af IRIS Group og professor Poul Houman Andersen i tæt samarbejde med erhvervshusene og Syddansk Universitet.
                    Den baserer sig på professor Thomas Ritters (CBS) forskning i forretningsmodeller. Udviklingen af testen er finansieret af
                    Industriens Fond.
                  </p>
                </div>
                <div class="col-md-6 mb-7">
                  <div class="card card-border">
                    <div class="card-header px-md-7">
                      <h2>Testens opbygning</h2>
                    </div>
                    <div class="card-text px-md-7">
                      <p>
                        Der er to dele i testen. Første del handler om at skabe et overblik over interne og eksterne faktorer, der kan skabe pres for
                        at justere forretningsmodellen. Anden del handler om at identificere de områder i jeres forretningsmodel, hvor der er rum for
                        forbedringer.
                      </p>
                      <ul>
                        <li>
                          Første del kan eventuelt springes over, hvis I allerede oplever at have styr på, hvad der udfordrer jeres nuværende
                          forretningsmodel, fx ændringer i markedet, ny teknologi eller pres for grøn omstilling. Vi anbefaler dog, at hele testen
                          gennemføres.
                        </li>
                        <li>
                          Når testen er gennemført, vil du blive præsenteret for mulige forbedringsområder i jeres forretningsmodel samt inspirerende
                          cases fra andre virksomheder, der på succesfuld vis har håndteret tilsvarende udfordringer i forretningsmodellen.
                        </li>
                        <li>
                          Resultaterne af testen sammenfattes også i en PDF-rapport, som I kan arbejde videre med, og som I evt. kan indsende digitalt
                          til jeres regionale erhvervshus med henblik på at få sparring.
                        </li>
                        <li>I rapporten bliver jeres svar også sammenlignet med svarene fra andre virksomheder i jeres branche.</li>
                      </ul>
                      <p>
                        Det skal understreges, at testen fungerer som en selvevaluering, hvor du/I kommer igennem de centrale dele i en
                        forretningsmodel. Resultatet kan derfor variere, alt efter hvem i virksomheden der udfylder testen.
                      </p>
                      <p>
                        Den er designet som et dialogværktøj, hvor værdien opstår ved, at I drøfter resultatet internt, med en konsulent i jeres
                        regionale erhvervshus eller med andre eksterne samarbejdspartnere.
                      </p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <div id="modal-method" class="fds-modal" aria-hidden="true" role="dialog" aria-modal="true" aria-labelledby="modal-id-1-title">
            <div class="modal-content">
              <div class="modal-header">
                <h2 id="modal-method-title" class="modal-title">Metode bag Udviklingskompasset</h2>
                <button class="modal-close function-link" data-modal-close>
                  <svg class="icon-svg" focusable="false" aria-hidden="true"><use xlink:href="#close"></use></svg>Luk
                </button>
              </div>
              <div class="modal-body">
                <p>
                  Testen er empirisk baseret. Den bygger på en samlet kortlægning af drivkræfter for forretningsudvikling og typiske udfordringer i
                  forretningsmodellen blandt ca. 200 SMV'er.
                </p>
                <p>
                  Kortlægningen fandt sted i forbindelse med erfaringsopsamlingen på Industriens Fonds genstartNU-program, som blev søsat i marts 2020
                  for at udvikle dansk erhvervsliv under corona-krisen. Erfaringsopsamlingen blev gennemført af IRIS Group i samarbejde med forskere
                  fra tre danske universiteter.
                </p>
                <p>
                  Udviklingskompasset tager afsæt i Alignment Squared-modellen, der er udviklet af professor Thomas Ritter fra CBS. Modellen er valgt,
                  fordi den er enkel og overskuelig, og fordi den har særlig fokus på sammenhænge og ubalancer mellem elementerne i
                  forretningsmodellen (hvor vores forskning har kortlagt, at SMV’er ofte har udfordringer).
                </p>
              </div>

              <div class="modal-footer">
                <div class="row">
                  <div class="col-12">
                    <div class="row align-items-center">
                      <div class="col-auto">
                        <button class="button" data-modal-close @click.prevent>Luk vindue</button>
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
              <div class="testclass2 col-lg-9 p-lg-4 px-lg-9 mb-0 px-xl-4">
                <p class="h6">{{ currentStep === section.steps.length ? 'Resultat' : 'Test' }}</p>

                <h1 class="h1">{{ section.headline }}</h1>
                <p v-if="(section.description || section.descriptionAlternative) && currentStep === 0" class="font-lead">
                  {{ currentSection === 'test2' && values.industry !== '' && maxStep > 0 ? section.descriptionAlternative : section.description }}
                </p>
                <p v-if="(section.description || section.resultIntro) && currentStep === section.steps.length" class="font-lead">
                  {{ section.resultIntro }}
                </p>
              </div>
              <div
                v-if="currentStep !== section.steps.length"
                :class="['testclass mb-7 pb-lg-7 px-lg-9 px-xl-4 pt-0 pt-lg-0', currentStep === 0 || currentStep === 1 ? 'col-lg-7' : 'col-lg-9']"
              >
                <button
                  v-if="currentStep === section.steps.length && currentSection === 'test1'"
                  class="button button-primary"
                  @click.prevent="
                    currentSection = 'test2';
                    currentStep = 0;
                    skipIndustrySelect = true;
                  "
                >
                  Gå til del 2: Hvor kan jeres forretningsmodel forbedres?
                </button>

                <div v-for="(step, stepIndex) of section.steps" :key="stepIndex">
                  <fieldset v-if="stepIndex === currentStep">
                    <div v-if="step.questionIntro" class="mb-8" v-html="step.questionIntro"></div>
                    <h2 v-if="step.headline" class="h2 mt-0">{{ step.headline }}</h2>
                    <p v-html="step.description"></p>
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
                            role="slider"
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
                      v-if="currentSection === 'test1' && currentStep + 1 === section.steps.length"
                      id="primaryButton"
                      class="button button-primary mt-7"
                      @click.prevent="handleSubmit(false)"
                    >
                      Fortsæt til del 2
                    </button>
                    <button
                      v-if="currentStep + 1 === section.steps.length"
                      id="primaryButton"
                      :class="['button mt-7', currentSection === 'test1' ? '' : 'button-primary']"
                      @click.prevent="handleSubmit(true)"
                    >
                      Se resultat
                    </button>
                    <button v-else id="primaryButton" class="button button-primary d-block mt-7" @click.prevent="goToNextStep()">
                      {{ currentStep > 0 ? 'Næste' : skipIndustrySelect ? 'Fortsæt' : 'Næste' }}
                    </button>

                    <button class="back-link d-block mt-3" @click.prevent="currentStep > 0 ? currentStep-- : (currentSection = 'frontpage')">
                      {{ currentStep > 0 ? 'Forrige' : 'Tilbage' }}
                    </button>
                  </fieldset>
                </div>
              </div>
              <div v-if="currentStep === 0 || currentStep === 1" :class="currentSection === 'test1' ? 'col-lg-5' : 'col-lg-4'">
                <figure class="figure">
                  <Image1 v-if="currentSection === 'test1'" />
                  <Image2 v-else />
                </figure>
              </div>

              <!-- Results start -->
              <div v-if="currentStep === section.steps.length" class="col-12">
                <div :class="`${currentSection === 'test1' ? 'bg-normal' : ''} full-width mt-3 p-lg-7`">
                  <div v-if="currentSection === 'test1'" class="container">
                    <div class="row">
                      <div class="col-12">
                        <h2 class="mt-0">Dit resultat</h2>
                      </div>
                      <div class="col-12">
                        <div class="row align-items-center">
                          <div class="col-auto">
                            <button class="button" @click.prevent="generateReport()">
                              <svg class="icon-svg" focusable="false" aria-hidden="true">
                                <use xlink:href="#file-download"></use></svg
                              >Download resultatet som PDF
                            </button>
                          </div>
                          <div class="col-auto">
                            <div
                              v-if="isDownloading"
                              class="spinner"
                              aria-label="Henter PDF"
                              :aria-busy="isDownloading"
                              style="margin: 16px auto; font-size: 6px"
                            />
                          </div>
                        </div>
                      </div>
                      <div class="col-12 col-xl-7">
                        <h3 class="h4">{{ section.resultPrimaryHeadline }}</h3>
                        <apexchart
                          v-if="response['test1']"
                          type="radar"
                          :options="radarOptions('test1')"
                          height="400"
                          :series="radarData('test1')"
                        ></apexchart>
                      </div>
                      <div class="col-xl-5">
                        <h3 class="h4">{{ section.resultSecondaryHeadline }}</h3>
                        <apexchart
                          v-if="response['test1'] && currentSection === 'test1'"
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
                        <p
                          v-if="response['test1'] && !response['test1'].scores[0].mean_score_industry"
                          class="align-text-center"
                          style="font-size: 14px; font-style: italic; margin-top: 0"
                        >
                          Din valgte branche har desværre på dette tidspunkt endnu for få besvarelser til at vise branchegennemsnittet
                        </p>
                      </div>
                    </div>
                  </div>
                  <div v-else-if="currentSection === 'test2'" class="container">
                    <div class="row">
                      <div class="col-12">
                        <div class="row align-items-center">
                          <div class="col-auto">
                            <button class="button" @click.prevent="generateReport()">
                              <svg class="icon-svg" focusable="false" aria-hidden="true">
                                <use xlink:href="#file-download"></use></svg
                              >Download resultatet som PDF
                            </button>
                          </div>
                          <div class="col-auto">
                            <div
                              v-if="isDownloading"
                              class="spinner"
                              aria-label="Henter PDF"
                              :aria-busy="isDownloading"
                              style="margin: 16px auto; font-size: 6px"
                            />
                          </div>
                        </div>
                      </div>
                      <div class="col-lg-7">
                        <h2>Dit resultat: Forslag til, hvor I kan styrke forretningsmodellen</h2>
                        <p>
                          Ud fra dine svar kunne der være et potentiale for at styrke forretningsmodellen på følgende områder – det mest oplagte
                          område nævnt først:
                        </p>
                        <ol v-if="response['test2'] && response['test2'].suggestions" class="suggestion-list">
                          <li v-for="(link, index) of response['test2'].suggestions.slice(0, 4)" :key="link.theme ? link.theme : index">
                            <div>
                              <p class="h4 m-0">{{ link.title ? link.title : '' }}</p>
                              <p class="mt-2">{{ link.description ? link.description : '' }}</p>
                              <a :href="link.linkUrl ? link.linkUrl : index" target="_blank"
                                >{{ `Få inspiration til, hvordan I kan ${link.title ? link.title.toLowerCase() : link.description.toLowerCase()}` }}
                                <svg class="icon-svg" focusable="false" aria-hidden="true">
                                  <use xlink:href="#open-in-new"></use>
                                </svg>
                              </a>
                            </div>
                          </li>
                        </ol>
                        <h2>Sådan kan du arbejde videre med forretningsmodellen</h2>
                        <p>
                          Der findes en række muligheder for at hente inspiration og sparring inden for forretningsmodeludvikling. Her finder du et
                          overblik over, hvordan I kan arbejde videre med jeres forretningsmodel.
                        </p>
                        <ul class="large-dots">
                          <li>
                            <p class="h4 mt-0">Få gratis og uvildig sparring hos et regionalt erhvervshus</p>
                            <p>
                              I kan hente gratis, uvildig sparring i jeres regionale erhvervshus. Lad en forretningsudvikler fra Erhvervshuset hjælpe
                              med at gå jeres testsvar igennem, konkretisere jeres udfordringer og udvikle en handlingsplan. De regionale erhvervshuse
                              kan også hjælpe jer med at finde frem til rådgivere eller værktøjer, som I kan trække på i arbejdet med at styrke
                              forretningsmodellen.
                            </p>
                          </li>
                          <li>
                            <p class="h4 mt-0">Drøft resultatet internt</p>
                            <p>
                              I kan også bruge testen til at drøfte internt, hvordan I kan udvikle virksomheden. Prøv fx at lade alle i ledelsen eller
                              bestyrelsen besvare testen. Drøft derefter forskelle og ligheder i jeres besvarelser.
                            </p>
                          </li>
                          <li>
                            <p class="h4 mt-0">Ræk ud til private rådgivere</p>
                            <p>
                              I kan forsøge at række ud til private rådgivere. Der findes både rådgivere med fokus på generel forretnings- og
                              strategiudvikling samt specialiserede rådgivere, der er eksperter inden for de forskellige dele af forretningsmodellen.
                            </p>
                          </li>
                        </ul>
                        <div class="card card-blue">
                          <div class="card-header pt-md-7 px-md-7">
                            <h2>Tag dit resultat med til dit regionale erhvervshus</h2>
                          </div>
                          <div class="card-text px-md-7">
                            <p>
                              Download dit resultat og tag det med til dit regionale erhvervshus. Testens resultater kan fungere som afsæt for
                              sparring om forretningsmodellen.
                            </p>
                          </div>
                          <div class="card-footer pb-4 px-4 pb-md-7 px-md-7">
                            <button class="button button-primary" data-module="modal" data-target="modal-contact" @click.prevent>
                              Kontakt dit regionale Erhvervshus
                            </button>
                          </div>
                        </div>
                        <div class="card card-blue">
                          <div class="card-header pt-0 px-md-7">
                            <h2>Download resultatet som PDF</h2>
                          </div>
                          <div class="card-text px-md-7">
                            <p>
                              Få et samlet overblik over dit resultat og dyk mere ned i, hvordan dine svar ser ud i sammenligning med andre
                              virksomheder i Danmark og branchen.
                            </p>
                          </div>
                          <div class="card-footer pb-4 px-4 pb-md-7 px-md-7">
                            <div class="row align-items-center">
                              <div class="col-auto">
                                <button class="button" @click.prevent="generateReport()">
                                  <svg class="icon-svg" focusable="false" aria-hidden="true">
                                    <use xlink:href="#file-download"></use></svg
                                  >Download resultatet som PDF
                                </button>
                              </div>
                              <div class="col-auto">
                                <div
                                  v-if="isDownloading"
                                  class="spinner"
                                  aria-label="Henter PDF"
                                  :aria-busy="isDownloading"
                                  style="margin: 16px auto; font-size: 6px"
                                />
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                      <div class="offset-lg-1 col-lg-4 mt-6 mt-2xl-0 pr-0">
                        <div class="card card-border">
                          <div class="card-header">
                            <h3 class="header-title h2">Tag dit resultat med til dit regionale erhvervshus</h3>
                          </div>
                          <div class="card-footer card-action">
                            <div class="action-buttons">
                              <div>
                                <button class="mb-4 button button-primary" data-module="modal" data-target="modal-contact" @click.prevent>
                                  Kontakt dit regionale erhvervshus
                                </button>
                              </div>
                              <div>
                                <a href="https://erhvervsfremmebestyrelsen.dk/tvaerkommunale-erhvervshuse" class="" target="_blank">
                                  Læs mere om de regionale erhvervshuse
                                  <svg class="icon-svg" focusable="false" aria-hidden="true">
                                    <use xlink:href="#open-in-new"></use></svg
                                ></a>
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>

                    <div id="modal-contact" class="fds-modal" aria-hidden="true" role="dialog" aria-modal="true" aria-labelledby="modal-id-1-title">
                      <div class="modal-content">
                        <form ref="contactForm" @submit.prevent="contactBusinessHouse">
                          <div class="modal-header">
                            <h2 id="modal-contact-title" class="modal-title">Send testresultatet til jeres regionale erhvervshus</h2>
                            <button class="modal-close function-link" data-modal-close>
                              <svg class="icon-svg" focusable="false" aria-hidden="true"><use xlink:href="#close"></use></svg>Luk
                            </button>
                          </div>
                          <div v-if="!emailIsSent" class="modal-body">
                            <p>
                              Her kan du skrive til jeres regionale erhvervshus, hvor dit testresultat automatisk sendes med. Derefter vil en
                              forretningsudvikler fra erhvervshuset kontakte dig og tilbyde gratis, uvildig sparring, der relaterer sig til
                              besvarelserne.
                            </p>
                            <div class="form-group">
                              <label class="form-label" for="businessHouse">Erhvervshus *:</label>
                              <select
                                id="businessHouse"
                                class="form-select"
                                name="businessHouse"
                                required
                                @input="
                                  updateModalValue('businessHouse', $event.target.value);
                                  selectedBusinessHouse = $event.target.value;
                                "
                              >
                                <option value="0" :disabled="contactFormValues.businessHouse">Vælg dit regionale erhvervshus...</option>
                                <option v-for="(businessHouse, index) of businessHouses" :key="index + 1" :value="`option${index + 1}`">
                                  {{ businessHouse.name }}
                                </option>
                              </select>
                            </div>
                            <div class="form-group">
                              <label class="form-label" for="navn">Din virksomheds navn *:</label>
                              <input
                                id="companyName"
                                required
                                class="form-input"
                                name="companyName"
                                type="text"
                                @input="updateModalValue('companyName', $event.target.value)"
                              />
                            </div>
                            <div class="form-group">
                              <label class="form-label" for="cvr">Din virksomheds CVR-nummer:</label>
                              <input
                                id="cvr"
                                class="form-input"
                                name="cvr"
                                type="text"
                                pattern="\d{8}"
                                title="Indtast 8-cifret CVR"
                                @input="updateModalValue('cvr', $event.target.value)"
                              />
                            </div>
                            <div class="form-group">
                              <label class="form-label" for="name">Dit navn *:</label>
                              <input
                                id="name"
                                class="form-input"
                                required
                                name="name"
                                type="text"
                                @input="updateModalValue('name', $event.target.value)"
                              />
                            </div>
                            <div class="row mt-5">
                              <div class="col-12 col-md-6">
                                <div class="form-group">
                                  <label class="form-label" for="email">Din e-mailadresse *:</label>
                                  <input
                                    id="email"
                                    class="form-input"
                                    :required="!contactFormValues.phone"
                                    name="email"
                                    type="email"
                                    @input="updateModalValue('email', $event.target.value)"
                                    @blur="validateEmail($event.target.value)"
                                  />
                                </div>
                              </div>
                              <div class="col-12 col-md-6">
                                <div class="form-group">
                                  <label class="form-label" for="phone">Dit telefonnummer *:</label>
                                  <input
                                    id="phone"
                                    class="form-input"
                                    :required="!contactFormValues.email"
                                    name="phone"
                                    type="text"
                                    title="Indtast 8-cifret telefonnummer"
                                    pattern="\d{8}"
                                    @input="updateModalValue('phone', $event.target.value)"
                                  />
                                </div>
                              </div>
                            </div>
                            <div class="form-group">
                              <label class="form-label" for="comments">Eventuelle kommentarer:</label>
                              <textarea
                                id="comments"
                                class="form-input"
                                name="comments"
                                maxlength="200"
                                @input="updateModalValue('comments', $event.target.value)"
                              ></textarea>
                            </div>
                          </div>

                          <div class="modal-footer">
                            <div class="row">
                              <div v-if="emailIsSent" class="col-12 mt-6">
                                <div role="alert" aria-atomic="true" class="alert mt-0 mb-8 alert-success">
                                  <div class="alert-body">
                                    <ul class="alert-text nobullet-list">
                                      <li>
                                        Tak for din indsendelse. Vi har sendt dit resultat videre til
                                        {{ businessHouses[contactFormValues.businessHouse.replace('option', '') - 1].name }}.
                                      </li>
                                    </ul>
                                  </div>
                                </div>
                              </div>

                              <div class="col-12">
                                <div class="row align-items-center">
                                  <!-- <div class="col-auto">
                                    <p>
                                      <a href="" target="_blank"
                                        >Når du anvender kontaktformularen samtykker du til behandling af dine personoplysninger. Læs om vores
                                        privatlivspolitik og dine rettigheder
                                        <svg class="icon-svg" focusable="false" aria-hidden="true">
                                          <use xlink:href="#open-in-new"></use></svg
                                      ></a>
                                    </p>
                                  </div> -->
                                  <div v-if="!emailIsSent" class="col-auto">
                                    <div class="form-group">
                                      <ul class="nobullet-list">
                                        <li>
                                          <input id="consent" type="checkbox" name="consent" value="true" class="form-checkbox" required />
                                          <label for="consent"
                                            >Klik for at samtykke til
                                            {{
                                              selectedBusinessHouse
                                                ? businessHouses[selectedBusinessHouse.replace('option', '') - 1].name + 's'
                                                : 'Erhvervshusets'
                                            }}
                                            behandling af dine personoplysninger.
                                            <a
                                              v-if="selectedBusinessHouse"
                                              :href="
                                                selectedBusinessHouse
                                                  ? businessHouses[selectedBusinessHouse.replace('option', '') - 1].cookielink
                                                  : ''
                                              "
                                              target="_blank"
                                              >Læs om erhvervshusets privatlivspolitik og dine rettigheder.<svg
                                                class="icon-svg"
                                                focusable="false"
                                                aria-hidden="true"
                                              >
                                                <use xlink:href="#open-in-new"></use></svg></a
                                          ></label>
                                        </li>
                                      </ul>
                                    </div>
                                  </div>
                                  <div class="col-auto">
                                    <button v-if="!emailIsSent" class="button button-primary">Indsend resultater til erhvervshuset</button>
                                    <button v-else class="button" data-modal-close @click.prevent>Luk vindue</button>
                                  </div>
                                  <div class="col-auto">
                                    <div
                                      v-if="isSending"
                                      class="spinner"
                                      aria-label="Sender e-mail"
                                      :aria-busy="isSending"
                                      style="margin: 16px auto; font-size: 6px"
                                    />
                                  </div>
                                </div>
                              </div>

                              <div v-if="emailError" class="col-12 mt-6">
                                <div role="alert" aria-atomic="true" class="alert mt-0 mb-8 alert-error">
                                  <div class="alert-body">
                                    <ul class="alert-text nobullet-list">
                                      <li>
                                        {{ emailError || 'Tak for din indsending' }}
                                      </li>
                                    </ul>
                                  </div>
                                </div>
                              </div>
                            </div>
                          </div>
                        </form>
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
                          Fortsæt til del 2
                        </button>
                      </div>
                    </div>
                  </div>
                </div>

                <button
                  v-if="currentStep > 0"
                  class="mx-lg-7 mx-xl-0 back-link d-block mt-5"
                  @click.prevent="
                    currentStep = 0;
                    currentSection = 'frontpage';
                  "
                >
                  Tag testen igen
                </button>
              </div>
            </div>
          </template>
        </div>
      </div>

      <!-- {{response}} -->
      <!-- to preview in browser: show-layout true and float-layout false -->
      <vue3-html2pdf
        ref="html2Pdf"
        :show-layout="false"
        :float-layout="true"
        :enable-download="false"
        :preview-modal="false"
        :pdf-quality="2"
        :manual-pagination="true"
        pdf-format="a4"
        pdf-orientation="portrait"
        :html-to-pdf-options="{
          margin: 20,
          enableLinks: true,
          image: { type: 'jpeg', quality: 0.75 },
          html2canvas: {
            dpi: 192,
            scale: 3,
            letterRendering: true,
            useCORS: true
          },
          jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
        }"
        @beforeDownload="beforeDownload($event)"
      >
        <template #pdf-content>
          <section>
            <section class="pdf-item">
              <!-- PAGE 1 -->
              <p class="h6">Rapport</p>
              <h1 class="h3 mt-0">Resultat af test af forretningsmodellen</h1>
              <p>Udskrevet d. {{ getDateString() }}</p>
              <table class="table table--dotted table--borderless mt-9">
                <tr>
                  <th>Virksomhedsnavn:</th>
                  <td style="vertical-align: middle; white-space: nowrap">{{ contactFormValues.companyName }}</td>
                </tr>
                <tr class="pt-4">
                  <th style="vertical-align: middle">Branche:</th>
                  <td>{{ values.industry }}</td>
                </tr>
              </table>
              <table class="table table--borderless table--blue" style="margin-top: 100px">
                <tr>
                  <td>
                    <p class="h6">Del 1</p>
                    <h1 class="h4 mt-0">Hvordan er presset på jeres forretningsmodel?</h1>
                    <p>
                      Testen viser, hvor I oplever forandringer i jeres forretningsbetingelser, der kan indebære et behov for at styrke eller retænke
                      forretningsmodellen.
                    </p>
                    <figure class="figure p-5">
                      <Image1 />
                    </figure>
                  </td>
                  <td>
                    <p class="h6">Del 2</p>
                    <h2 class="h4 mt-0">Hvor kan jeres forretningsmodel forbedres?</h2>
                    <p>Testen viser, hvor I selv vurderer, at der er forbedringsmuligheder i jeres nuværende forretningsmodel.</p>
                    <figure class="figure p-5">
                      <Image2 />
                    </figure>
                  </td>
                </tr>
              </table>
            </section>
            <div class="html2pdf__page-break" />
            <section class="pdf-item">
              <!-- PAGE 2 -->
              <p class="h6">Opsummering af resultater</p>
              <div>
                <h2 class="h4 mt-2">Del 1: Hvordan er presset på jeres forretningsmodel?</h2>
                <h3 class="h6">{{ sections[0].resultPrimaryHeadline }}</h3>
                <p class="form-hint">Skala på 1-10, hvor 1 er lille/intet pres og 10 er stort pres</p>
                <apexchart
                  v-if="response['test1']"
                  type="radar"
                  :options="radarOptions('test1', true)"
                  width="650px"
                  :series="radarData('test1')"
                  style="margin-bottom: -30px !important; margin-top: -30px !important"
                ></apexchart>
                <p v-else class="mt-12 h2">Del 1 er ikke gennemført</p>
              </div>

              <div>
                <h2 class="h4 mt-0">Del 2: Hvor kan jeres forretningsmodel forbedres?</h2>
                <p>Ud fra dine svar ser der ud til at være et potentiale for at styrke jeres forretningsmodel på nedenstående områder.</p>
                <ol v-if="response['test2'] && response['test2'].suggestions" class="suggestion-list">
                  <li v-for="(link, index) of response['test2'].suggestions.slice(0, 4)" :key="link.theme ? link.theme : index">
                    <div>
                      <p class="h4 m-0">{{ link.title ? link.title : '' }}</p>
                      <p class="mt-2 mb-2">{{ link.description ? link.description : '' }}</p>
                    </div>
                  </li>
                </ol>
                <p v-else class="mt-12 h2">Del 2 er ikke gennemført</p>
              </div>
            </section>
            <div class="html2pdf__page-break" />
            <section class="pdf-item">
              <!-- PAGE 3 -->
              <div v-if="response['test1']">
                <div
                  v-for="(sectionGroup, sectionGroupIndex) of [
                    { sections: sections[0].steps.slice(2, 5) },
                    { sections: sections[0].steps.slice(5, 7) }
                  ]"
                  :key="sectionGroupIndex"
                >
                  <p class="h6">Resultat - del 1 {{ sectionGroupIndex > 0 ? ' - fortsat' : '' }}</p>
                  <h2 class="h4 mt-2">Dine svar</h2>
                  <p>Dine svar på 18 udsagn om presset på jeres forretningsmodel på en skala fra 1-10, hvor 1 er meget uenig og 10 er meget enig.</p>
                  <table class="table--invisible">
                    <tbody v-for="(section, sectionIndex) of sectionGroup.sections" :key="sectionIndex">
                      <tr v-if="section.headline" class="">
                        <td>
                          <figure class="m-0 mt-3 mr-4">
                            <IconInternal v-if="section.id == 'internal'" />
                            <IconMarket v-if="section.id == 'market'" />
                            <IconSupplychain v-if="section.id == 'supplychain'" />
                            <IconSustainability v-if="section.id == 'sustainability'" />
                            <IconTechnology v-if="section.id == 'technology'" />
                          </figure>
                        </td>
                        <td colspan="2">
                          <div class="underlined-headline-wrapper">
                            <h3 class="h5 mt-0 underlined-headline">{{ section.headline }}</h3>
                          </div>
                        </td>
                        <td class="h6 align-text-center">
                          <CircledAverage
                            :percentage="(calculateSectionAverage(section, false) / 10) * 100"
                            :value="calculateSectionAverage(section, true)"
                          />
                        </td>
                      </tr>
                      <tr v-for="(question, questionIndex) of section.questions" :key="questionIndex">
                        <td></td>
                        <td :class="`${questionIndex < section.questions.length - 1 && 'dotted'}`">
                          <p class="m-0">{{ question.label }}</p>
                        </td>
                        <td :class="`${questionIndex < section.questions.length - 1 && 'dotted'} px-4`">
                          <GradientLine :percentage="(values[question.name] / 10) * 100" :width="200" />
                        </td>
                        <td :class="`h6 align-text-center ${questionIndex < section.questions.length - 1 && 'dotted'}`">
                          {{ values[question.name] }}
                        </td>
                      </tr>
                    </tbody>
                  </table>
                  <div class="html2pdf__page-break" />
                </div>
              </div>
              <div v-else>
                <p class="h6">Resultat - del 1</p>
                <h2 class="h4 mt-2">Dine svar</h2>
                <p>Dine svar på 18 udsagn om presset på jeres forretningsmodel på en skala fra 1-10, hvor 1 er meget uenig og 10 er meget enig.</p>
                <p class="mt-12 h2">Del 1 er ikke gennemført</p>
                <div class="html2pdf__page-break" />
              </div>
            </section>
            <section class="pdf-item">
              <p class="h6">Resultat - del 2</p>
              <h2 class="h4 mt-2">Hvor kan jeres forretningsmodel forbedres?</h2>
              <p>
                Dine svar på 24 udsagn om hvor din forretningsmodel kan forbedres på en skala fra 1-10, hvor 1 er meget uenig og 10 er meget enig.
              </p>
              <table v-if="values && response['test2']" class="table--invisible table--fixed">
                <tr v-for="i in 3" :key="i">
                  <td v-for="j in 3" :key="j" style="vertical-align: top" class="p-4">
                    <figure v-if="i == 2 && j == 2"><Image2 /></figure>
                    <Test2Cell v-else :col-index="j" :row-index="i" :steps="sections[1].steps" :values="values" />
                  </td>
                </tr>
              </table>
              <p v-else class="mt-12 h2">Del 2 er ikke gennemført</p>
            </section>
            <div class="html2pdf__page-break" />
            <section class="pdf-item">
              <p class="h6">Resultat</p>
              <h2 class="h4 mt-2">Sådan kan du arbejde videre med forretningsmodellen</h2>
              <p>
                Der findes en række muligheder for at hente inspiration og sparring inden for forretningsmodeludvikling. Her finder du et overblik
                over, hvordan I kan arbejde videre med jeres forretningsmodel.
              </p>
              <h3 class="h5">Få gratis og uvildig sparring hos et regionalt erhvervshus</h3>
              <p>
                I kan hente gratis, uvildig sparring i jeres regionale erhvervshus. Lad en forretningsudvikler fra Erhvervshuset hjælpe med at gå
                jeres testsvar igennem, konkretisere jeres udfordringer og udvikle en handlingsplan.
              </p>
              <p>
                Du kan finde dit regionale erhvervshus samt kontaktoplysninger her:
                <br /><a href="https://erhvervsfremmebestyrelsen.dk/tvaerkommunale-erhvervshuse" target="_blank"
                  >https://erhvervsfremmebestyrelsen.dk/tvaerkommunale-erhvervshuse</a
                >
              </p>
              <h3 class="h5">Bliv inspireret af cases med relevans for jer</h3>
              <p>
                Ud fra dine svar, fik du foreslået fire områder, hvor der kan være potentiale for at styrke forretningsmodellen. På
                Virksomhedsguiden.dk kan du læse om virksomheder, der på succesfuld vis har arbejdet med at styrke disse områder:
              </p>
              <ol v-if="response['test2'] && response['test2'].suggestions">
                <li v-for="(link, index) of response['test2'].suggestions.slice(0, 4)" :key="link.theme ? link.theme : index">
                  <a :href="link.linkUrl ? link.linkUrl : index"
                    >{{ `Få inspiration til, hvordan I kan ${link.title ? link.title.toLowerCase() : link.description.toLowerCase()}` }}
                  </a>
                </li>
              </ol>
              <p v-else class="mt-4 h2">Del 2 er ikke gennemført</p>

              <h3 class="h5">Drøft resultatet internt</h3>
              <p>
                I kan også bruge testen til at drøfte internt, hvordan I kan udvikle virksomheden. Prøv fx at lade alle i ledelsen eller bestyrelsen
                besvare testen. Drøft derefter forskelle og ligheder i jeres besvarelser.
              </p>
              <h3 class="h5">Ræk ud til private rådgivere</h3>
              <p>
                I kan forsøge at række ud til private rådgivere. Der findes både rådgivere med fokus på generel forretnings- og strategiudvikling samt
                specialiserede rådgivere, der er eksperter inden for de forskellige dele af forretningsmodellen.
              </p>
            </section>
            <div class="html2pdf__page-break" />
            <section class="pdf-item">
              <h2 class="h1" style="margin-top: 200px">Bilag</h2>
              <p style="font-size: 14px; line-height: 1.4; max-width: 37ch">
                Sammenlign dit resultat med andre virksomheder i din branche og alle virksomheder
              </p>
            </section>
            <div class="html2pdf__page-break" />
            <section class="pdf-item">
              <div
                v-for="(sectionGroup, sectionGroupIndex) of [
                  { sections: sections[0].steps.slice(2, 5), headline: 'Del 1 - ' + sections[0].headline, section: 'test1' },
                  { sections: sections[0].steps.slice(5, 7), headline: 'Del 1 - ' + sections[0].headline + ' - fortsat', section: 'test1' },
                  { sections: sections[1].steps.slice(2, 6), headline: 'Del 2 - ' + sections[1].headline, section: 'test2' },
                  { sections: sections[1].steps.slice(6, 10), headline: 'Del 2 - ' + sections[1].headline + ' - fortsat', section: 'test2' }
                ]"
                :key="sectionGroupIndex"
              >
                <div v-if="response[sectionGroup.section]">
                  <p class="h6">Bilag</p>
                  <h2 class="h4 mt-2">Gennemsnitssvar for branchen og alle virksomheder</h2>
                  <p class="h6 mb-4 mt-2">{{ sectionGroup.headline }}</p>
                  <hr />
                  <table class="table--invisible">
                    <tbody v-for="(section, sectionIndex) of sectionGroup.sections" :key="sectionIndex">
                      <tr style="transform: translateY(10px) translateX(20px)">
                        <td></td>
                        <td></td>
                        <td class="diagonal"><strong>Dine svar</strong></td>
                        <td class="diagonal">
                          Din branche
                          <span v-if="response[sectionGroup.section] && !response[sectionGroup.section].scores[0].mean_score_industry">*</span>
                        </td>
                        <td class="diagonal">Alle svar</td>
                      </tr>
                      <tr v-if="section.headline" class="">
                        <td class="pr-4">
                          <figure class="m-0 mt-3">
                            <IconInternal v-if="section.id == 'internal'" />
                            <IconMarket v-else-if="section.id == 'market'" />
                            <IconSupplychain v-else-if="section.id == 'supplychain'" />
                            <IconSustainability v-else-if="section.id == 'sustainability'" />
                            <IconTechnology v-else-if="section.id == 'technology'" />
                            <IconValue v-else-if="section.id == 'value'" />
                            <IconRessources v-else-if="section.id == 'resources'" />
                            <IconCustomers v-else-if="section.id == 'customers'" />
                            <IconSales v-else-if="section.id == 'sales'" />
                            <IconScales v-else />
                          </figure>
                        </td>
                        <td colspan="4">
                          <div class="underlined-headline-wrapper mr-0">
                            <h3 class="h5 mt-0 underlined-headline">{{ section.pdfHeadline ? section.pdfHeadline : section.headline }}</h3>
                          </div>
                        </td>
                        <!-- <td class="h6 align-text-center"><strong>Dine svar</strong></td>
                    <td class="h6 align-text-center">Din branche</td>
                    <td class="h6 align-text-center">Alle svar</td> -->
                      </tr>
                      <tr v-for="(question, questionIndex) of section.questions" :key="questionIndex">
                        <td></td>
                        <td :class="`${questionIndex < section.questions.length - 1 && 'dotted'}`">
                          <p class="m-0">{{ question.label }}</p>
                        </td>
                        <td :class="`${questionIndex < section.questions.length - 1 && 'dotted'} px-4`">
                          <div class="valueContainer ml-4">
                            <p class="h5 mt-0 mb-2">{{ values[question.name] }}</p>
                            <GradientLine :percentage="(values[question.name] / 10) * 100" :width="30" />
                          </div>
                        </td>
                        <td :class="`${questionIndex < section.questions.length - 1 && 'dotted'} px-4`">
                          <div class="valueContainer ml-4">
                            <p class="h5 mt-0 mb-2">
                              {{ getMeansValueFromResponse(sectionGroup.section, question.name, 'mean_industry', true) }}
                            </p>
                            <GradientLine
                              :percentage="(getMeansValueFromResponse(sectionGroup.section, question.name, 'mean_industry', false) / 10) * 100"
                              :width="30"
                            />
                          </div>
                        </td>
                        <td :class="`${questionIndex < section.questions.length - 1 && 'dotted'} px-4`">
                          <div class="valueContainer ml-4">
                            <p class="h5 mt-0 mb-2">
                              {{ getMeansValueFromResponse(sectionGroup.section, question.name, 'mean_all', true) }}
                            </p>
                            <GradientLine
                              :percentage="(getMeansValueFromResponse(sectionGroup.section, question.name, 'mean_all', false) / 10) * 100"
                              :width="30"
                            />
                          </div>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                  <p
                    v-if="response[sectionGroup.section] && !response[sectionGroup.section].scores[0].mean_score_industry"
                    style="font-style: italic; max-width: none; margin-bottom: -10px"
                  >
                    * Din valgte branche har desværre på dette tidspunkt endnu for få besvarelser til at vise branchegennemsnittet
                  </p>
                  <div class="html2pdf__page-break" />
                </div>
              </div>
            </section>
            <!-- PDF Content Here -->
          </section>
        </template>
      </vue3-html2pdf>
    </form>
  </div>
</template>

<script lang="ts">
export interface DataPoint {
  category: string;
  mean_all: number;
  mean_answers: number;
  mean_industry: number;
  question: string;
  text: string;
  value: number;
}

export interface Question {
  label: string;
  name: string;
  options: string[];
}
export interface Section {
  description: string;
  headline: string;
  id: string;
  questions: Question[];
}

export interface PDFOptions {
  enableLinks: boolean;
  html2canvas: { dpi: number; scale: number; letterRendering: boolean; useCORS: boolean };
  image: { type: string; quality: number };
  jsPDF: { unit: string; format: string; orientation: string };
  margin: number;
}

import axios, { AxiosError, AxiosResponse } from 'axios';
import moment from 'moment';
import Image1 from './Image1.vue';
import Image2 from './Image2.vue';
import SvgIcons from './SvgIcons.vue';
import IconInternal from './IconInternal.vue';
import IconMarket from './IconMarket.vue';
import IconSupplychain from './IconSupplychain.vue';
import IconTechnology from './IconTechnology.vue';
import IconSustainability from './IconSustainability.vue';
import IconScales from './IconScales.vue';
import IconValue from './IconValue.vue';
import IconRessources from './IconRessources.vue';
import IconCustomers from './IconCustomers.vue';
import IconSales from './IconSales.vue';
import Logo from './Logo.vue';
import GradientLine from './GradientLine.vue';
import CircledAverage from './CircledAverage.vue';
import Test2Cell from './Test2Cell.vue';
import VueApexCharts from 'vue3-apexcharts';
import Vue3Html2pdf from 'vue3-html2pdf';
import { getCurrentInstance } from 'vue';

import * as DKFDS from 'dkfds';
import * as DataEvent from '@erst-vg/piwik-event-wrapper';

const app = getCurrentInstance();

console.log('# APP ', app);

export interface Variant {
  // mailgunApiKey: string;
  navn: string;
  aktiv: boolean;
  parametre: {
    parameternavn: string;
    parametervaerdi: string;
  }[];
}

export default {
  name: 'Applikation',
  components: {
    apexchart: VueApexCharts,
    Image1,
    Image2,
    Logo,
    IconInternal,
    IconMarket,
    IconSupplychain,
    IconTechnology,
    IconSustainability,
    SvgIcons,
    GradientLine,
    Test2Cell,
    CircledAverage,
    Vue3Html2pdf,
    IconScales,
    IconValue,
    IconRessources,
    IconCustomers,
    IconSales
  },
  props: {
    variant: {
      type: Object as () => Variant,
      default: null,
      required: false
    }
  },

  data() {
    return {
      response: {},
      error: '',
      emailError: '',
      errors: {},
      isLoading: false,
      isDownloading: false,
      isSending: false,
      currentStep: 0, // initial value 0
      maxStep: 0,
      currentSection: 'frontpage', // initial value frontpage - possible values 'frontpage', 'test1', 'test2'
      skipIndustrySelect: false,
      pdfIsReady: false,
      sendEmail: false,
      emailIsSent: false,

      apiBaseUrl: 'https://vg-api.irisgroup.dk/api',
      defaultOptions: [
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
      ],
      industries: [
        { label: 'Industri', value: 'Industri' },
        { label: 'Bygge og anlæg', value: 'Bygge og anlæg' },
        { label: 'Handel', value: 'Handel' },
        { label: 'Transport', value: 'Transport' },
        { label: 'Videnservice', value: 'Videnservice' },
        { label: 'Kommunikation og reklame', value: 'Kommunikation og reklame' },
        { label: 'Operationel service', value: 'Operationel service' },
        { label: 'Oplevelseserhverv', value: 'Oplevelseserhverv' },
        { label: 'Andet', value: 'Andet' }
      ],
      defaultDescription: 'Angiv hvor enig du er i de enkelte udsagn, hvor 1= meget uenig og 10= meget enig.',
      values: {
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
      },
      contactFormValues: {},
      selectedBusinessHouse: '',
      mailgunBaseUrl: 'https://api.eu.mailgun.net/v3',
      mailgunDomain: 'mailgun.irisgroup.dk',
      pdfBlob: '',
      businessHouses: [
        {
          name: 'Erhvervshus Hovedstaden',
          email: 'ftr@ehhs.dk',
          cookielink: 'https://ehhs.dk/content/ydelser/erhvervshusets-politikker/ab931645-7084-4d15-97ae-8f7f5d4865a7/#button-07770'
        },
        {
          name: 'Erhvervshus Nordjylland',
          email: 'mdr@ehnj.dk',
          cookielink: 'https://ehnj.dk/content/ydelser/persondatapolitik/9cfc69b1-7462-4ce7-89a4-bb33ed217129/'
        },
        {
          name: 'Erhvervshus Midtjylland',
          email: 'pe@erhvervshusmidtjylland.dk',
          cookielink: 'https://erhvervshusmidtjylland.dk/content/ydelser/privatlivspolitik/a594090a-ef39-429e-b3e4-c9db8c9f8107/'
        },
        {
          name: 'Erhvervshus Sydjylland',
          email: 'kho@ehsyd.dk',
          cookielink: 'https://ehsyd.dk/content/ydelser/persondatapolitik/f6920b7f-e231-4a59-9ca8-f713eda2de26/'
        },
        {
          name: 'Erhvervshus Fyn',
          email: 'larsb@erhvervshusfyn.dk, stna@erhvervshusfyn.dk',
          cookielink: 'https://ehfyn.dk/content/ydelser/persondata/dfa1816b-12e1-4657-b4d1-cddcbc492fcd/'
        },
        {
          name: 'Erhvervshus Sjælland',
          email: 'kko@ehsj.dk',
          cookielink: 'https://ehsj.dk/content/ydelser/privatlivspolitik/4522de1e-cd30-449d-a568-c676b1b6b885/'
        }
      ]
    };
  },

  computed: {
    sessionId: function () {
      return this.generateId(32);
    },

    sections: function () {
      return [
        {
          id: 'test1',
          headline: 'Hvordan er presset på din forretningsmodel?',
          resultIntro:
            'Nedenfor finder du et overblik over presset på din virksomheds forretningsmodel. Du kan også se, hvordan andre virksomheder inden for samme branche gennemsnitligt oplever presset. Vi anbefaler, at du også tager testens anden del, der viser dig, hvor din forretningsmodel potentielt kan styrkes.',
          resultPrimaryHeadline: 'Pres for forretningsmodellen fordelt på områder',
          resultSecondaryHeadline: 'Samlet billede af presset på forretningsmodellen',
          description:
            'Svar på en række udsagn om din virksomhed inden for fem områder og få en indikation af, hvordan presset er på din forretningsmodel.',
          steps: [
            {
              description: `<p>Der kan være mange årsager til, at en forretningsmodel kommer under pres, eller at der opstår nye muligheder, der gør det relevant at justere forretningsmodellen.</p>
          <p>Formålet med denne del af testen er at give jer en indikation af, om der kan være behov for at efterse forretningsmodellen.</p>

          <p>Testen baserer sig på en stor undersøgelse blandt virksomheder, der succesfuldt har styrket forretningsmodellen. Den viser, at drivkræfter bag forretningsmodeludvikling typisk falder inden for fem hovedområder.</p>

          <p>I testen beder vi dig derfor tage stilling til en række udsagn knyttet til hvert af de fem områder.</p>`
            },
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
                  label: 'Vi har svært ved at realisere de opstillede mål i virksomheden, fx i forhold til omsætning',
                  options: this.defaultOptions
                }
              ],
              id: 'internal'
            },
            {
              headline: 'Marked og efterspørgsel',
              description: this.defaultDescription,
              questions: [
                {
                  name: 'market1',
                  label: 'Vi oplever et stigende prispres og konkurrence på omkostninger vores branche',
                  options: this.defaultOptions
                },
                {
                  name: 'market2',
                  label: 'Vi oplever en stigende konkurrence på kvalitet og innovation i vores branche',
                  options: this.defaultOptions
                },
                {
                  name: 'market3',
                  label: 'Branchen/markedet påvirkes af nye trends, der skaber forandringer i behov og efterspørgsel hos vores kunder/slutbrugere',
                  options: this.defaultOptions
                }
              ],
              id: 'market'
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
                  label: 'Ny teknologi rummer store potentialer for at udvikle bedre løsninger i vores branche',
                  options: this.defaultOptions
                },
                {
                  name: 'technology3',
                  label: 'Digitale teknologier rummer store potentialer for at styrke salg og demonstration af løsninger i vores branche',
                  options: this.defaultOptions
                }
              ],
              id: 'technology'
            },
            {
              headline: 'Leverandører og samarbejdspartnere',
              description: this.defaultDescription,
              questions: [
                {
                  name: 'supplychain1',
                  label: 'Håndtering af risici på leverandørsiden bliver en vigtigere konkurrencefaktor i vores branche',
                  options: this.defaultOptions
                },
                {
                  name: 'supplychain2',
                  label: 'Der sker forandringer i leverandørnetværket i vores branche, fx i retning af færre led eller konsolidering ',
                  options: this.defaultOptions
                },
                {
                  name: 'supplychain3',
                  label: 'Vores kunder/slutkunder lægger i stigende grad vægt på profil og sammensætning af vores leverandører og samarbejdspartnere',
                  options: this.defaultOptions
                }
              ],
              id: 'supplychain'
            },
            {
              headline: 'Bæredygtighed og grøn omstilling',
              description: this.defaultDescription,
              questions: [
                {
                  name: 'sustainability1',
                  label: 'Vi oplever et stigende fokus hos vores kunder/slutbrugere på grøn omstilling og klimavenlighed',
                  options: this.defaultOptions
                },
                {
                  name: 'sustainability2',
                  label:
                    'Der er et stort potentiale i vores branche/forsyningskæde for omstilling i retning af cirkulære principper eller lavere CO2-aftryk',
                  options: this.defaultOptions
                },
                {
                  name: 'sustainability3',
                  label: 'Vi har en stærk ambition i virksomheden om at bidrage yderligere til den grønne omstilling',
                  options: this.defaultOptions
                }
              ],
              id: 'sustainability'
            }
          ]
        },
        {
          id: 'test2',
          headline: 'Hvor kan jeres forretningsmodel forbedres?',
          description:
            'Svar på en række udsagn om din virksomhed inden for forretningsmodellens fire hovedelementer. Til sidst får du et samlet resultat, der kan give dig inspiration til, hvor og hvordan din forretningsmodel kan styrkes.',
          descriptionAlternative:
            'Svar på en række udsagn om din virksomhed inden for forretningsmodellens hovedområder – værditilbud, kunder, salg og kommunikation samt ressourcer og processer. Til sidst får du et samlet resultat, der både viser dig, hvordan presset er på din virksomhed, samt giver dig inspiration til, hvordan din forretningsmodel kan styrkes.',
          resultIntro:
            'Nedenfor finder du bud på, hvor jeres forretningsmodel kan forbedres, samt inspiration til, hvordan I kan arbejde videre med udvikling af forretningsmodellen.',
          resultPrimaryHeadline: 'Dit resultat: Forslag til, hvor I kan styrke forretningsmodellen',
          resultSecondaryHeadline: 'Ud fra dine svar ser der især ud til at være potentiale for at styrke din forretningsmodel på følgende områder',
          steps: [
            {
              description: `<p>Jeres forretningsmodel kan inddeles i fire hovedelementer, som vist i figuren til højre::</p>
              <ol>
              <li>værditilbud</li>
              <li>kunder</li>
              <li>salg og kommunikation</li>
              <li>ressourcer og processer</li>
              </ol>

              <p>På de næste fire sider beder vi dig tage stilling til en række udsagn knyttet til hvert af de fire hovedelementer. Derefter bedes du forholde dig til udsagn om, hvordan sammenhængen er mellem forretningsmodellens elementer (illustreret ved hjørnerne i figuren til højre).</p>`
            },
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
              pdfRowIndex: 1, // out of 3
              pdfColIndex: 2, // out of 3
              id: 'value',
              description: this.defaultDescription,
              questions: [
                {
                  name: 'value1',
                  label:
                    'Vi har behov for at blive klarere på, hvordan vi skal differentiere os fra vores konkurrenter (fx med hensyn til design, services og pris)',
                  options: this.defaultOptions
                },
                {
                  name: 'value2',
                  label: 'Vi har et potentiale for at indtænke nye trends (fx bæredygtighed) i vores løsninger',
                  options: this.defaultOptions
                },
                {
                  name: 'value3',
                  label:
                    'Vi har et potentiale for at udvikle nye betalingsmodeller for vores løsninger (fx abonnementer eller betaling efter forbrug) ',
                  options: this.defaultOptions
                }
              ]
            },
            {
              headline: 'Kunder',
              pdfRowIndex: 2, // out of 3
              pdfColIndex: 3, // out of 3
              id: 'customers',
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
              pdfRowIndex: 3, // out of 3
              pdfColIndex: 2, // out of 3
              id: 'sales',
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
                  label: 'Vi har udfordringer med at strukturere og følge op på vores salgsarbejde',
                  options: this.defaultOptions
                }
              ]
            },
            {
              headline: 'Ressourcer og processer',
              pdfRowIndex: 2, // out of 3
              pdfColIndex: 1, // out of 3
              id: 'resources',
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
                  label: 'Vi har udfordringer på ressourcesiden (fx adgang til kvalificeret arbejdskraft, kapital eller komponenter/råvarer)',
                  options: this.defaultOptions
                },
                {
                  name: 'resources3',
                  label: 'Vi har behov for at styrke vores forsyningskæde eller interne logistik',
                  options: this.defaultOptions
                }
              ]
            },
            {
              headline: 'Samspil mellem værditilbud og kunder',
              pdfHeadline: 'Samspil ml. værditilbud og kunder',
              pdfRowIndex: 1, // out of 3
              pdfColIndex: 3, // out of 3
              questionIntro: `<p class="font-lead mt-0">At udvikle en god forretningsmodel kræver ikke blot, at de fire elementer i forretningsmodellen fungerer godt og understøtter jeres mål. Det er også vigtigt, at der er god sammenhæng mellem elementerne – fx at jeres værditilbud matcher behovet hos vigtige kunder.</p>
<p class="font-lead mt-0">I den resterende del af testen bedes du tage stilling til en række udsagn om samspillet mellem de fire elementer i forretningsmodellen.</p>`,
              description: this.defaultDescription,
              questions: [
                {
                  name: 'valuecustomer1',
                  label: 'Vi kan blive bedre til at målrette vores løsninger til specifikke kundetyper/-segmenter',
                  options: this.defaultOptions
                },
                {
                  name: 'valuecustomer2',
                  label: 'Vi har behov for bedre overblik over hvilke kunder, der køber hvilke løsninger',
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
              headline: 'Samspil mellem kunder og salg og kommunikation',
              pdfHeadline: 'Samspil ml. kunder og salg/kommunikation',
              pdfRowIndex: 3, // out of 3
              pdfColIndex: 3, // out of 3
              description: this.defaultDescription,
              questions: [
                {
                  name: 'customersales1',
                  label: 'Vi  har behov for at tilpasse vores kommunikation og salgsindsats til forskellige kundetyper',
                  options: this.defaultOptions
                },
                {
                  name: 'customersales2',
                  label: 'Vi mangler kanaler/værktøjer til at identificere og komme i dialog med potentielle kunder',
                  options: this.defaultOptions
                },
                {
                  name: 'customersales3',
                  label:
                    'Vi har behov for at skabe bedre indgange for kunderne, fx i forhold til at give os feedback eller efterspørge opfølgende service',
                  options: this.defaultOptions
                }
              ]
            },
            {
              headline: 'Samspil mellem salg og kommunikation og ressourcer',
              pdfHeadline: 'Samspil ml. ressourcer mv. og salg/kommunikation',
              pdfRowIndex: 3, // out of 3
              pdfColIndex: 1, // out of 3
              description: this.defaultDescription,
              questions: [
                {
                  name: 'salesresources1',
                  label: 'Vi har behov for nye kompetencer i forhold til at styrke demonstration og salg af vores løsninger',
                  options: this.defaultOptions
                },
                {
                  name: 'salesresources2',
                  label: 'Vi har behov for at styrke samspillet mellem salg og andre funktioner i virksomheden',
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
              headline: 'Samspil mellem ressourcer og værditilbud',
              pdfHeadline: 'Samspil ml. ressourcer mv. og værditilbud',
              pdfRowIndex: 1, // out of 3
              pdfColIndex: 1, // out of 3
              description: this.defaultDescription,
              questions: [
                {
                  name: 'resourcesvalue1',
                  label: 'Vi har behov for at tilføre ny viden/teknologi for at udvikle bedre løsninger',
                  options: this.defaultOptions
                },
                {
                  name: 'resourcesvalue2',
                  label: 'Vi har behov for at løfte HR og kompetenceudvikling for at konkurrere i vores marked',
                  options: this.defaultOptions
                },
                {
                  name: 'resourcesvalue3',
                  label: 'Vi har for høje omkostninger i forhold til vores priser og kvaliteten af vores løsninger',
                  options: this.defaultOptions
                }
              ]
            }
          ]
        }
      ];
    },

    mailgunApiKey: function () {
      return this.variant?.parametre[0].parametervaerdi ?? process.env.VUE_APP_MAILGUN_API_KEY;
    }
  },
  watch: {
    currentStep(newStep: number, oldStep: number) {
      window.scrollTo(0, 0);
      this.maxStep = this.currentStep > this.maxStep ? this.currentStep - 1 : this.maxStep;
      const { hash, pathname } = window.location;
      const nextUrl = pathname + '#' + this.currentSection + '-' + newStep;
      DataEvent.emitNaesteEvent(this, nextUrl);
      window.location.hash = this.currentSection + '-' + newStep;
    },
    currentSection(newSection: string, oldSection: string) {
      window.scrollTo(0, 0);
      this.maxStep = this.currentStep > this.maxStep ? this.currentStep - 1 : this.maxStep;
      const { hash, pathname } = window.location;
      let nextUrl;
      if (newSection === 'frontpage') {
        nextUrl = pathname + '#start';
      } else {
        nextUrl = pathname + '#' + newSection + '-' + this.currentStep;
      }
      DataEvent.emitNaesteEvent(this, nextUrl);
      window.location.hash = newSection === 'frontpage' ? 'start' : newSection + '-' + this.currentStep;
    }
  },
  mounted() {
    DKFDS.init();
  },
  created() {
    window.location.hash = 'start';
    window.addEventListener('hashchange', this.updateStepFromHash);
  },
  updated() {
    if (this.currentSection !== 'frontpage') {
      document.addEventListener('keydown', event => {
        if (event.key === 'Enter') {
          event.stopImmediatePropagation();
          const button = document.querySelector('.applikation-container #primaryButton') as HTMLElement;
          button.click();
        }
      });
    }
    DKFDS.init();
  },

  destroyed() {
    window.removeEventListener('hashchange', this.updateStepFromHash);
  },
  methods: {
    getMeansValueFromResponse(step: string, questionName: string, meansKey: string, format: boolean) {
      if (!this.response[step] || !this.response[step].question_means) {
        return 'N/A';
      }

      const currentQuestion = this.response[step].question_means.find((questionItem: DataPoint) => questionName === questionItem.question);

      if (currentQuestion && currentQuestion[meansKey]) {
        return format
          ? currentQuestion[meansKey].toLocaleString('da-DK', { minimumFractionDigits: 1, maximumFractionDigits: 1 })
          : currentQuestion[meansKey];
      } else {
        return 'N/A';
      }
    },
    async generateReport() {
      this.isDownloading = true;
      const html2pdfComponent = this.$refs.html2Pdf;
      html2pdfComponent.generatePdf();
      this.emitDownloadEvent();
    },
    async beforeDownload({ html2pdf, options, pdfContent }: { html2pdf: any; options: PDFOptions; pdfContent: HTMLElement }) {
      const pdf = await html2pdf().set(options).from(pdfContent).toPdf().get('pdf');
      const totalPages = pdf.internal.getNumberOfPages();
      for (let i = 1; i <= totalPages; i++) {
        const pageWidth = pdf.internal.pageSize.getWidth();
        const pageHeight = pdf.internal.pageSize.getHeight();
        pdf.setPage(i);
        pdf.setFontSize(8);
        pdf.setTextColor('#454545');
        pdf.text(i + '   /   ' + totalPages, pageWidth - 20, pageHeight - 7);
        const imgData =
          'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAB3AAAAPkCAYAAABYxK8AAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAgY0hSTQAAeiYAAICEAAD6AAAAgOgAAHUwAADqYAAAOpgAABdwnLpRPAAAAAZiS0dEAAAAAAAA+UO7fwAAAAlwSFlzAAAASAAAAEgARslrPgAAgABJREFUeNrs3XucZOlZH/bfe6q6e2b2ftN1dVuttOi6YiXtipVWXhAIEOJiYzs4dsCBJBA7UmwHk6AFYyfOJxYSMcTXhGCbXHDATmI7xtiAAQtkrhLGCKRdJJAEu6vd1V7n0jNdVefNH6equrqmZncu3X16Zr/f3Z7T5+nTp57zVvVMdT31vG8CAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA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AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIDbz/8P3JpCQk5V84IAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjEtMTAtMDZUMjA6MDc6NDcrMDg6MDCqqZffAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIxLTEwLTA2VDIwOjA3OjQ3KzA4OjAw2/QvYwAAAABJRU5ErkJggg==';

        pdf.addImage(imgData, 'PNG', 10, pageHeight - 24, 40, 21);
      }
      // });
      if (!this.sendEmail) {
        pdf.save('udviklingskompasset_resultat.pdf');
      } else {
        const pdfBlob = await pdf.output('blob');
        this.pdfBlob = pdfBlob;
        this.contactBusinessHouse();
      }
      this.isDownloading = false;
      this.pdfIsReady = true;
    },

    updateValue(key: string, value: number | string) {
      this.$emit('input', (this.values[key] = value));
    },

    updateModalValue(key: string, value: number | string) {
      this.contactFormValues[key] = value;
    },

    goToNextStep() {
      if (this.validate()) {
        if (this.currentSection === 'test2' && this.currentStep === 0 && this.response.test1) {
          this.currentStep = 2;
        } else {
          this.currentStep++;
        }
      }
    },

    goToStep(step: number) {
      if (step < this.currentStep) {
        this.currentStep = step;
      } else {
        if (this.validate()) {
          this.currentStep = step;
        }
      }
    },
    handleSubmit(showResults = true) {
      if (!this.validate()) {
        return;
      }
      this.isLoading = true;
      const answers = {} as Record<string, unknown>;
      this.sections
        .find((section: Section) => section.id === this.currentSection)
        .steps.map((step: Section) => {
          if (!step.questions) {
            return;
          }
          step.questions.forEach((question: Question) => {
            if (question.name === 'industry') {
              return;
            }
            answers[question.name] = this.values[question.name];
          });
        });

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
        .then((rsp: AxiosResponse[]) => {
          this.isLoading = false;
          if (!rsp[0].data.error) {
            this.isLoading = false;
            if (rsp.length > 1) {
              this.response[this.currentSection] = { ...rsp[0].data, suggestions: rsp[1].data };
            } else {
              this.response[this.currentSection] = rsp[0].data;
            }
          }

          if (showResults) {
            this.currentStep++;
          } else {
            this.currentStep = 0;
            this.currentSection = 'test2';
          }
        })
        .catch((error: AxiosError) => {
          this.isLoading = false;
          this.error = 'Noget gik galt. Prøv venligst igen.';
        });
    },

    contactBusinessHouse() {
      if (!this.validateEmail()) {
        return;
      }

      this.sendEmail = true;
      this.isSending = true;

      if (this.pdfBlob.length === 0) {
        this.generateReport();
        return;
      }

      let content = '';
      content += `<p><b>Virksomhedens navn:</b> <br/>${this.contactFormValues.companyName}</p>`;
      content += `<p><b>CVR-nummer:</b> <br/>${this.contactFormValues.cvr}</p>`;
      content += `<p><b>Navn:</b> <br/>${this.contactFormValues.name}</p>`;
      content += `<p><b>E-mail:</b> <br/>${this.contactFormValues.email}</p>`;
      content += `<p><b>Telefonnummer:</b> <br/>${this.contactFormValues.phone}</p>`;
      content += `<p><b>Kommentarer:</b> <br/>${this.contactFormValues.comments}</p>`;
      const formData = new FormData();
      formData.append('attachment', this.pdfBlob, 'udviklingskompasset_resultat.pdf');
      axios
        .post(`${this.mailgunBaseUrl}/${this.mailgunDomain}/messages`, formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          },
          params: {
            from: 'Iris Group <mail@mailgun.irisgroup.dk>',
            to: this.businessHouses[this.contactFormValues.businessHouse.replace('option', '') - 1].email,
            // to: ['cd@irisgroup.dk', 'xenia.j@adviceas.dk'],
            // to: 'xenia.j@adviceas.dk, xenia@franet.dk',
            subject: 'Testresultater fra Udviklingskompasset',
            html: content
          },
          auth: {
            username: 'api',
            password: this.mailgunApiKey
          }
        })
        .then(
          response => {
            this.isSending = false;
            this.emailIsSent = true;
            this.sendEmail = false;
            this.$refs.contactForm[0].reset();
            this.emailError = '';
          },
          reject => {
            this.isSending = false;
            this.sendEmail = false;
            this.emailError = 'Noget gik galt. Prøv venligst igen.';
          }
        );
      this.emitCTAClickEvent();
    },

    generateId(length: number) {
      let result = '';
      const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      const charactersLength = characters.length;
      for (let i = 0; i < length; i++) {
        result += characters.charAt(Math.floor(Math.random() * charactersLength));
      }

      return result;
    },

    radarOptions(section: string, pdf = false) {
      if (!this.response || !this.response[section]?.category_means) {
        return null;
      }

      const chartColors = {
        textColor: '#1A1A1A'
      };

      const categories = this.response[section].category_means
        .filter((dataPoint: DataPoint) => dataPoint.category)
        .map((dataPoint: DataPoint) => {
          if (dataPoint.category.length > 15) {
            const wordList = dataPoint.category.split(' ');
            const half = Math.ceil(wordList.length / 2);
            const firstHalf = wordList.slice(0, half);
            const secondHalf = wordList.slice(half, wordList.length);
            return [firstHalf.join(' '), secondHalf.join(' ')];
          } else {
            return dataPoint.category;
          }
        });
      return {
        chart: {
          id: 'radar',
          fontFamily: pdf ? 'IBMPlexSans, sans-serif' : 'IBMPlexSans, system',
          toolbar: {
            show: false
          }
        },
        dataLabels: {
          enabled: false
        },
        stroke: {
          width: 1
        },
        plotOptions: {
          radar: {
            size: pdf ? 130 : 150,
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
              fontSize: pdf ? 10 : 12
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
              fontSize: pdf ? 10 : 12
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
          fontSize: pdf ? 10 : 14,
          itemMargin: {
            horizontal: 8,
            vertical: 8
          },
          offsetY: pdf ? -50 : 0
        }
      };
    },

    columnOptions(pdf = false) {
      if (!this.response || !this.response[this.currentSection]) {
        return null;
      }

      const categories = this.response[this.currentSection].scores[0].mean_score_industry
        ? ['Din virksomhed', 'Branchen', 'Alle virksomheder']
        : ['Din virksomhed', 'Alle virksomheder'];

      return {
        chart: {
          id: 'bar',
          fontFamily: pdf ? 'IBMPlexSans, sans-serif' : 'IBMPlexSans, system',
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
              fontSize: pdf ? 11 : 13
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
              fontSize: pdf ? 10 : 12
            }
          }
        },
        legend: {
          show: false
        }
      };
    },

    validate() {
      this.errors = {};

      if (!this.sections.find((section: Section) => section.id === this.currentSection).steps[this.currentStep].questions) {
        return true;
      }

      this.sections
        .find((section: Section) => section.id === this.currentSection)
        .steps[this.currentStep].questions.forEach((question: Question, index: number) => {
          if (this.values[question.name] === 0) {
            this.errors[question.name] = {
              errorSummary: 'Angiv hvor enig du er i dette udsagn på en skala fra 1-10',
              errorMessage: 'Angiv hvor enig du er i dette udsagn.',
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
    },

    validateEmail() {
      if (/^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/.test(this.contactFormValues.email)) {
        this.emailError = '';
        return true;
      } else {
        this.emailError = 'Indtast en gyldig emailadresse';
        return false;
      }
    },

    updateStepFromHash() {
      const { hash } = window.location;
      if (hash) {
        if (hash === '#start') {
          this.currentSection = 'frontpage';
        } else {
          const hashParts = hash.split('-');
          if (hashParts.length === 2) {
            this.currentSection = this.removeHash(hashParts[0]);
            this.currentStep = parseInt(this.removeHash(hashParts[1]), 10);
          }
        }
      }
      DataEvent.emitPageViewEvent(this);
    },
    removeHash(hash: string) {
      return hash.replaceAll('#', '');
    },

    // Data opsamlingsmetoder
    emitDownloadEvent() {
      DataEvent.emitDownloadEvent(this, 'udviklingskompasset_resultat.pdf', 'download diagnose results');
    },
    emitCTAClickEvent() {
      DataEvent.emitCTAClickEvent(this, 'contactBusinessHouse', 'CTA data');
    },

    columnData() {
      if (!this.response[this.currentSection]) {
        return [];
      }
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
    },

    radarData(section: string) {
      if (!this.response[section]) {
        return [];
      }
      const categoryMeans = this.response[section].category_means.filter((dataPoint: DataPoint) => dataPoint.category);
      if (this.response[section].category_means[0].mean_industry) {
        return [
          {
            name: 'Din virksomhed',
            data: categoryMeans.map((dataPoint: DataPoint) => dataPoint.mean_answers.toFixed(2))
          },
          {
            name: 'Branchen',
            data: categoryMeans.map((dataPoint: DataPoint) => dataPoint.mean_industry.toFixed(2))
          },
          {
            name: 'Alle virksomheder',
            data: categoryMeans.map((dataPoint: DataPoint) => dataPoint.mean_all.toFixed(2))
          }
        ];
      }
      return [
        {
          name: 'Din virksomhed',
          data: categoryMeans.map((dataPoint: DataPoint) => dataPoint.mean_answers.toFixed(2))
        },
        {
          name: 'Alle virksomheder',
          data: categoryMeans.map((dataPoint: DataPoint) => dataPoint.mean_all.toFixed(2))
        }
      ];
    },
    calculateSectionAverage(section: Section, format: boolean) {
      let sum = 0 as number;
      let average = 0 as number;
      section.questions.forEach((question: Question) => {
        sum += this.values[question.name];
      });
      average = sum / section.questions.length;
      return format ? average.toLocaleString('da-DK', { minimumFractionDigits: 1, maximumFractionDigits: 1 }) : average;
    },

    getDateString() {
      return new Date().toLocaleDateString('da-DK', { year: 'numeric', month: 'long', day: 'numeric' });
    }
  }
};
</script>
<style lang="scss" scoped>
@import '../styles/components/_applikation.scss';
</style>
