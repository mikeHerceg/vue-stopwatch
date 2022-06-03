<script setup lang="ts">
import PrimaryButton from '@/components/Primary-Button.vue';
import {ref, watch} from 'vue'

const count = ref<string>('00:00:00')
const isCounting = ref<boolean>(false)
const timeStamps = ref<string[]>([])

const toggleStart = () =>{ 
  isCounting.value = !isCounting.value
}

const handleReset = () =>{
  isCounting.value = false
  count.value = '00:00:00'
}

const handleTimeStamp = () =>{
  console.log(timeStamps.value)
 timeStamps.value.push(count.value)
}

const removeTimeStamp = (timeStamp:string) =>{
  timeStamps.value = timeStamps.value.filter(stamp => stamp !== timeStamp)
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
    <div class="timer">
        <h1>
        {{count}}
        </h1>
        <PrimaryButton :onClick="toggleStart">
          {{ isCounting ? 'stop' : 'start' }}
        </PrimaryButton>
        <PrimaryButton :onClick="handleReset">
          reset
        </PrimaryButton>
    </div>
    <div v-if="isCounting || timeStamps.length" class="time-stamps">
        <div  v-if="timeStamps.length">
        <h2>time stamps</h2>
        <ul >
          <li v-for="timeStamp in timeStamps" :key="timeStamp">
            {{ timeStamp }} 
            <PrimaryButton :onClick="()=>removeTimeStamp(timeStamp)">
              remove
            </PrimaryButton>
          </li>
        </ul>
        </div>
       
         <PrimaryButton :onClick="handleTimeStamp" v-if="isCounting">
          time Stamp 
        </PrimaryButton>
    </div>
    
    
  </main>
</template>

<style lang="scss">
  @import './assets/base.scss';
  body{
    display: flex;
    align-items: center;
    justify-content: center;
    main {
      display: flex;
      align-items: center;
      *{
        margin: 10px
      }
    }
  }
</style>
