<template>
  <div id="app">

  <svg width=20 height=20 stroke="black">
    <!-- lines -->
    <line x1="0" y1="0" x2="20" y2="0"/>
    <line x1="0" y1="10" x2="20" y2="10" />
    <line x1="0" y1="20" x2="20" y2="20" />

    <!-- notes -->
    <NoteBulb :note="note" x="3" y="4" @click="doAlert"/>
  </svg>

&nbsp;&nbsp;
    <select v-model="note.type">
      <option :value="1">Whole</option>
      <option :value="1/2">half</option>
      <option :value="1/4">quarter</option>
      <option :value="1/8">eighth</option>
      <option :value="1/16">sixteenth</option>
    </select>
    
    
   
    <br /><br />
    
  
    <select v-model="keyOffset">
      <option v-for="note in 12" :value="note - 1">{{getNote(note - 1).name}}</option>
    </select>
    <table>
      <tr v-for="step in scale" @click="playTone(getFrequency(step.offset + keyOffset))" :style="{ opacity: step.isHalf ? .5 : ''}">
        <td>{{step.name}}</td>
        <td>{{getNote(step.offset + keyOffset).name}}</td>
      </tr>
    </table>
  </div>

  <br />

  <svg width=200 height=80 stroke="black">
    <!-- lines -->
    <line x1="0" y1="0" x2="200" y2="0"/>
    <line x1="0" y1="10" x2="200" y2="10" />
    <line x1="0" y1="20" x2="200" y2="20" />
    <line x1="0" y1="30" x2="200" y2="30" />

    <!-- notes -->
    <NoteBulb v-for="step, i in scale.filter(s => !s.isHalf)" :note="{ type: 1 }" :x="i * 20" :y="34 + (-5 * getNote(step.offset + keyOffset).staffPos)" @click="doAlert"/>
  </svg>



</template>

<script setup>
import { ref, reactive } from 'vue'; 
import NoteBulb from './components/NoteBulb.vue';

const doAlert = () => {
  alert('hi')
}
  const note = reactive({
    type: 1,
    stemDirection: 'up',
    stemLength: undefined,
    // dotted: false,
  });
  
const keyOffset = ref(0);
const middleCHz = 261.63;
const getFrequency = noteNumber => middleCHz * Math.pow(2, noteNumber / 12);

const scale = [
  {
    name: 'do',
    offset: 0,
  },
  {
    name: 'di',
    isHalf: true,
    offset: 1,
  },
  {
    name: 're',
    offset: 2,
  },
  {
    name: 'ri',
    isHalf: true,
    offset: 3,
  },
  {
    name: 'mi',
    offset: 4,
  },
  {
    name: 'fa',
    offset: 5,
  },
  {
    name: 'fi',
    isHalf: true,
    offset: 6,
  },
  {
    name: 'sol',
    offset: 7,
  },
  {
    name: 'si',
    isHalf: true,
    offset: 8,
  },
  {
    name: 'la',
    offset: 9,
  },
  {
    name: 'li',
    isHalf: true,
    offset: 10,
  },
  {
    name: 'ti',
    offset: 11,
  },
  {
    name: 'do',
    offset: 12,
  },
]

const notes = [
  {
    name: 'C',
    staffPos: 0,
  },
  {
    isHalf: true,
    name: 'C#',
  },
  {
    name: 'D',
    staffPos: 1,
  },
  {
    isHalf: true,
    name: 'D#',
  },
  {
    name: 'E',
    staffPos: 2,
  },
  {
    name: 'F',
    staffPos: 3,
  },
  {
    isHalf: true,
    name: 'F#',
  },
  {
    name: 'G',
    staffPos: 4,
  },
  {
    isHalf: true,
    name: 'G#',
  },
  {
    name: 'A',
    staffPos: 5,
  },
  {
    isHalf: true,
    name: 'A#',
  },
  {
    name: 'B',
    staffPos: 7,
  },
]

// resolves any +/- integer to a note relative middle C at 0
const getNote = noteNumber => notes[(12 + (noteNumber % 12)) % 12];
  
// create web audio api context
var audioCtx = new(window.AudioContext || window.webkitAudioContext)();

function playTone(frequency, duration) {
  // create Oscillator node
  var oscillator = audioCtx.createOscillator();

  oscillator.frequency.value = frequency; // value in hertz
  oscillator.connect(audioCtx.destination);
  oscillator.start();    

  setTimeout(
  function() {
    oscillator.stop();
  }, duration);
}
  
  
</script>

<!-- Use preprocessors via the lang attribute! e.g. <style lang="scss"> -->
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
}

a,
button {
  color: #4fc08d;
}

button {
  background: none;
  border: solid 1px;
  border-radius: 2em;
  font: inherit;
  padding: 0.75em 2em;
}
</style>