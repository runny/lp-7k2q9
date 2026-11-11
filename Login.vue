<!--
  PaySauce login screen, five layout options.
  PrimeVue 4 components (InputText, Button, Select). Styling lives in login.css, same file the
  HTML prototype uses, so what Gary sees in the browser is what this renders.
  Pick the option with the `option` prop ("A" to "E"). Drop the option tag before shipping.
-->
<template>
  <div class="ps-login" :class="['opt-' + option, { 'has-sky': sky, 'has-card': card }]">
    <div v-if="sky" class="sky" aria-hidden="true"><i class="s1"></i><i class="s2"></i><i class="s3"></i><i class="s4"></i><i class="s5"></i><i class="s6"></i></div>

    <!-- top bar: logo and, for D and E, the country switch -->
    <header class="top" :class="{ 'top-inline': option === 'E' }">
      <a class="brand" href="#" aria-label="PaySauce">
        <img v-if="logo === 'script'" class="brand-script" :src="scriptLogo" alt="PaySauce">
        <span v-else class="brand-lockup">
          <img class="brand-mark" :src="auMark" alt="">
          <img class="brand-word" :src="auWordmark" alt="PaySauce">
        </span>
      </a>
      <Select v-if="regionSwitch" v-model="region" :options="regions" optionLabel="name" class="region" aria-label="Country">
        <template #value="{ value }">
          <span class="region-value"><i class="globe" aria-hidden="true"></i>{{ value.name }}</span>
        </template>
        <template #dropdownicon><i class="chev" aria-hidden="true"></i></template>
      </Select>
    </header>

    <main class="stage">
      <form class="card" @submit.prevent="$emit('submit', email)">
        <h1 class="title">{{ title }}</h1>
        <label class="label" for="email">Email<i>*</i></label>
        <InputText id="email" v-model="email" type="email" placeholder="Email" class="field" autocomplete="email" />
        <Button type="submit" label="Next" class="next" />
        <p class="foot">Don't have an account? <a href="#">Sign up</a></p>
      </form>
    </main>

    <span class="option-tag">Option {{ option }}</span>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import InputText from 'primevue/inputtext';
import Button from 'primevue/button';
import Select from 'primevue/select';
import scriptLogo from './assets/paysauce-script-red.svg';
import auMark from './assets/au-mark.svg';
import auWordmark from './assets/au-wordmark.svg';

const props = defineProps({ option: { type: String, default: 'A' } });
defineEmits(['submit']);

// What differs between the five options. Everything else is shared.
const specs = {
  A: { sky: true,  card: true,  logo: 'script', regionSwitch: false },
  B: { sky: false, card: false, logo: 'script', regionSwitch: false },
  C: { sky: true,  card: true,  logo: 'script', regionSwitch: false },
  D: { sky: true,  card: true,  logo: 'script', regionSwitch: true  },
  E: { sky: true,  card: true,  logo: 'lockup', regionSwitch: true  },
};
const { sky, card, logo, regionSwitch } = specs[props.option] || specs.A;

const regions = [{ name: 'Australia', code: 'au' }, { name: 'New Zealand', code: 'nz' }];
const region = ref(regions[0]);
const email = ref('');

// The phone designs say just "Login"; the desktop ones say where you are.
const mq = matchMedia('(max-width: 639px)');
const mobile = ref(mq.matches);
const onChange = e => (mobile.value = e.matches);
onMounted(() => mq.addEventListener('change', onChange));
onUnmounted(() => mq.removeEventListener('change', onChange));
const title = computed(() => (mobile.value ? 'Login' : 'Login to PaySauce Australia'));
</script>

<style src="./login.css"></style>
