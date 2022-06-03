<script setup lang="ts">
import PrimaryButton from '@/components/Primary-Button.vue';
import {ref, watch} from 'vue'

const count = ref<string>('00:00:00')
const isCounting = ref<boolean>(false)


const handleClick = () =>{ 
  isCounting.value = !isCounting.value
}


let hr = 0;
let min = 0;
let sec = 0;

function timerCycle() {
  
  if ( isCounting.value == true) {
   
    sec = sec + 1;

    if (sec == 60) {
      min = min + 1;
      sec = 0;
    }
    if (min == 60) {
      hr = hr + 1;
      min = 0;
      sec = 0;
    }

    if (sec < 10 || sec == 0) {
      sec = 0 + sec;
    }
    if (min < 10 || min == 0) {
      min = 0 + min;
    }
    if (hr < 10 || hr == 0) {
      hr = 0 + hr;
    }

    count.value = hr + ':' + min + ':' + sec;

    setTimeout(timerCycle, 1000);
  }
}

watch(isCounting, timerCycle)

</script>

<template>
  <main>
     {{count}}
    <PrimaryButton :onClick="handleClick">
      {{ isCounting ? 'stop' : 'start' }}
    </PrimaryButton>
  </main>
</template>

<style lang="scss">
  @import './assets/base.scss';
</style>
