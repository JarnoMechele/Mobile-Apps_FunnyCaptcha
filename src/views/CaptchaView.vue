<script setup lang="ts">
  import FunnyCaptcha from '@/components/FunnyCaptcha.vue';
  import { ref } from 'vue';

  const showCaptcha = ref(true);
  const isCaptchaCorrect = ref(false);

  const processCaptcha = function(result: boolean){
    console.log(`Captcha = ${result}`);
    isCaptchaCorrect.value = result;
    showCaptcha.value = false; 
  }

</script>
<template>
  <div>
    <FunnyCaptcha v-if="showCaptcha" @correct="processCaptcha(true)" @incorrect="processCaptcha(false)" :seconds="10"/>
    <div v-else>
      <p v-if="isCaptchaCorrect">ooofff at least you are sentient</p>
      <p v-else>Oh sorry, no place for robots here</p>
    </div>
  </div>
</template>