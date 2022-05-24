<!-- Samling af SVG ikoner som bruges af sandkasse-applikationen. Er der behov for flere ikoner, bør de tilføjes her. -->
<template>
  <div class="hide-base-svg">
    <svg xmlns="http://www.w3.org/2000/svg">
      <symbol id="home" viewBox="0 0 24 24">
        <path d="M0 0h24v24H0V0z" fill="none" />
        <path fill="currentColor" d="M12 5.69l5 4.5V18h-2v-6H9v6H7v-7.81l5-4.5M12 3L2 12h3v8h6v-6h2v6h6v-8h3L12 3z" />
      </symbol>
      <symbol id="attach-file" viewBox="0 0 24 24">
        <path d="M0 0h24v24H0V0z" fill="none" />
        <path
          fill="currentColor"
          d="M16.5 6v11.5c0 2.21-1.79 4-4 4s-4-1.79-4-4V5c0-1.38 1.12-2.5 2.5-2.5s2.5 1.12 2.5 2.5v10.5c0 .55-.45 1-1 1s-1-.45-1-1V6H10v9.5c0 1.38 1.12 2.5 2.5 2.5s2.5-1.12 2.5-2.5V5c0-2.21-1.79-4-4-4S7 2.79 7 5v12.5c0 3.04 2.46 5.5 5.5 5.5s5.5-2.46 5.5-5.5V6h-1.5z"
        />
      </symbol>
      <symbol id="xla-chat" viewBox="0 0 24 24">
        <path d="M0 0h24v24H0V0z" fill="none" />
        <path
          fill="currentColor"
          d="M4 4h16v12H5.17L4 17.17V4m0-2c-1.1 0-1.99.9-1.99 2L2 22l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2H4zm2 10h8v2H6v-2zm0-3h12v2H6V9zm0-3h12v2H6V6z"
        />
      </symbol>
      <symbol id="open-in-new" viewBox="0 0 24 24">
        <path d="M0 0h24v24H0V0z" fill="none" />
        <path
          d="M19 19H5V5h7V3H5c-1.11 0-2 .9-2 2v14c0 1.1.89 2 2 2h14c1.1 0 2-.9 2-2v-7h-2v7zM14 3v2h3.59l-9.83 9.83 1.41 1.41L19 6.41V10h2V3h-7z"
        />
      </symbol>
      <symbol id="arrow-drop-down" viewBox="0 0 24 24">
        <path d="M0 0h24v24H0V0z" fill="none" />
        <path d="M7 10l5 5 5-5H7z" />
      </symbol>
      <symbol id="done" viewBox="0 0 24 24">
        <path d="M0 0h24v24H0V0z" fill="none" />
        <path d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z" />
      </symbol>
      <symbol id="file-download" viewBox="0 0 24 24">
        <g><rect fill="none" height="24" width="24" /></g>
        <g>
          <path d="M18,15v3H6v-3H4v3c0,1.1,0.9,2,2,2h12c1.1,0,2-0.9,2-2v-3H18z M17,11l-1.41-1.41L13,12.17V4h-2v8.17L8.41,9.59L7,11l5,5 L17,11z" />
        </g>
      </symbol>
    </svg>
  </div>
</template>

<script lang="ts">
export default {
  name: 'SvgIcons'
};
</script>
