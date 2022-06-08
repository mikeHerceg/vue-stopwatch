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

    const transform = (value:number) => {
      if (value < 10 || value == 0) {
        return '0' + value.toString()
      }
      return value.toString()     
    }

    time.value =  {
        hr: transform(hr),
        min:transform(min),
        sec:transform(sec)
    };

    setTimeout(timerCycle, 1000);
  }
}

watch(isCounting, timerCycle)

</script>

<template>
  <main>
    <div class="timer">
        <div class="timer-container">
            <h1>
            {{time.hr}}:{{time.min}}:{{time.sec}}
            </h1>
            <div class="button-group">
              <div class="button-group_row">
                <PrimaryButton :onClick="toggleStart" :class="isCounting ? 'timer-button_stop' : 'timer-button_start'">
                  {{ isCounting ? 'stop' : 'start' }}
                </PrimaryButton>
                <PrimaryButton :onClick="handleReset">
                  reset
                </PrimaryButton>
              </div>
              <div class="button-group_row">
                <PrimaryButton 
                  :onClick="handleTimeStamp" 
                  :disabled="isCounting?false:true"
                >
                  Create Time Stamp 
                </PrimaryButton>
              </div>
            </div>
           
        </div>
    </div>
    
    <div class="column">
      <div v-if="isCounting || timeStamps.length" class="time-stamps">
              <div  v-if="timeStamps.length">
              <h1>Time Stamps</h1>
              <ul>
                <li v-for="timeStamp in timeStamps" :key="timeStamp">
                  {{ timeStamp }} 
                  <PrimaryButton :onClick="()=>removeTimeStamp(timeStamp)">
                    x
                  </PrimaryButton>
                </li>
              </ul>
              </div>
            
             
        </div>
    </div>
    
    
    
  </main>
</template>

<style lang="scss">
  @import './assets/base.scss';
  @import './assets/variables.scss';
    ul{
      margin: 0;
      padding: 0;
      list-style: none;
    }
    main {
      height: 100vh;
      width:100vw;

      .timer{
        background: var( --color-background-soft);
        z-index: 10;
        position: sticky;
        top:0;
        left: 0;
        padding:var(--spacer);
        margin-bottom:var(--spacer);
        border-bottom:2px solid gray;
        width: 100%;
        display: flex;
        justify-content: center;

        &-container{
          min-width: 150px;
          text-align: center;
        }

        &-button{
          &_start{
            background: lightseagreen;
          }
          &_stop{
            background: lightcoral;
          }
        }
        
      }
    }
    .button-group{
      width: 100%;      
      &_row {     
        margin-bottom:var(--spacer); 
        display: flex;
        justify-content: space-around;
        * {
          flex-grow: 1;
        }
      }
    }
  
</style>
