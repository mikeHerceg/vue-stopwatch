<script setup lang="ts">
import PrimaryButton from '@/components/Primary-Button.vue';
import {ref, watch} from 'vue'

interface Time {
  hr:string,
  min:string,
  sec:string
}
const initialTime:Time = {hr:'00',min:'00',sec:'00'}
const time = ref<Time>(initialTime)
const isCounting = ref<boolean>(false)
const timeStamps = ref<string[]>([])

const toggleStart = () =>{ 
  isCounting.value = !isCounting.value
}

const handleReset = () =>{
  isCounting.value = false
  time.value = initialTime
}

const handleTimeStamp = () =>{
  const {hr,min, sec} = time.value
  timeStamps.value.push(`${hr}:${min}:${sec}`)
}

const removeTimeStamp = (timeStamp:string) =>{
  timeStamps.value = timeStamps.value.filter(stamp => stamp !== timeStamp)
}



function timerCycle() {
  
  let hr = +time.value.hr;
  let min = +time.value.min;
  let sec = +time.value.sec;

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

    time.value =  {
        hr:hr.toString(),
        min:min.toString(),
        sec:sec.toString()
    };

    setTimeout(timerCycle, 1000);
  }
}

watch(isCounting, timerCycle)

</script>

<template>
  <main>
    <div class="column timer">
        <div >
            <h1>
            {{time.hr}}:{{time.min}}:{{time.sec}}
            </h1>
            <PrimaryButton :onClick="toggleStart">
              {{ isCounting ? 'stop' : 'start' }}
            </PrimaryButton>
            <PrimaryButton :onClick="handleReset">
              reset
            </PrimaryButton>
        </div>
    </div>
    
    <div class="column">
      <div v-if="isCounting || timeStamps.length" class="time-stamps">
              <div  v-if="timeStamps.length">
              <h1>Time Stamps</h1>
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
                Create Time Stamp 
              </PrimaryButton>
        </div>
    </div>
    
    
    
  </main>
</template>

<style lang="scss">
  @import './assets/base.scss';
    ul{
      margin: 0;
      padding: 0;
      list-style: none;
    }
    main {
      display: flex;
      align-items: stretch;
      height: 100vh;
      width:100vw;
      *{
        margin: 8px
      }
      .column {
          flex-grow: 1;
          width:50%;
          display: flex;
          align-items: center;
          justify-content: center;
      }
  }
</style>
