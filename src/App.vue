<template>
  <div id="app">

    <svg width=20 height=20 stroke="black">
      <!-- lines -->
      <line x1="0" y1="0" x2="20" y2="0" />
      <line x1="0" y1="10" x2="20" y2="10" />
      <line x1="0" y1="20" x2="20" y2="20" />

      <!-- notes -->
      <NoteHead :note="note" x="3" y="4" @click="doAlert" />
    </svg>

    &nbsp;&nbsp;
    <select v-model="note.type">
      <option :value="1">Whole</option>
      <option :value="1 / 2">half</option>
      <option :value="1 / 4">quarter</option>
      <option :value="1 / 8">eighth</option>
      <option :value="1 / 16">sixteenth</option>
    </select>



    <br /><br />


    <select v-model="keyOffset">
      <option v-for="note in 12" :value="note - 1">{{ getNote(note - 1).name }}</option>
    </select>

    <br /><br />

    <svg width=400 height=60 stroke="black">
      <!-- lines -->
      <line x1="0" y1="0" x2="400" y2="0" />
      <line x1="0" y1="10" x2="400" y2="10" />
      <line x1="0" y1="20" x2="400" y2="20" />
      <line x1="0" y1="30" x2="400" y2="30" />
      <line x1="0" y1="40" x2="400" y2="40" />

      <!-- notes -->
      <template v-for="step, i in scale">
        <NoteHead :note="{ type: 1, note: getNote(step.offset + keyOffset) }" :x="i * 30"
          :y="44 + (-5 * tonePos(step.offset + keyOffset))" @click="doAlert" />
      </template>
    </svg>

    <div style="display: flex; justify-content: space-around; width: 390px">
      <div v-for="step in scale" @click="playTone(getFrequency(step.offset + keyOffset))"
        :style="{ opacity: step.isHalf ? .5 : '', width: '30px', textAlign: 'center' }">
        <div>{{ step.name }}</div>
        <div>{{ getNote(step.offset + keyOffset).name }}</div>
    </div>
    </div>


  </div>

</template>

<script setup>
import { ref, reactive } from 'vue';
import NoteHead from './components/NoteHead.vue';

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
const getFrequency = toneNumber => middleCHz * Math.pow(2, toneNumber / 12);

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
    name: 'C#',
    isHalf: true,
    staffPos: 0,
  },
  {
    name: 'D',
    staffPos: 1,
  },
  {
    name: 'D#',
    isHalf: true,
    staffPos: 1,
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
    name: 'F#',
    isHalf: true,
    staffPos: 3,
  },
  {
    name: 'G',
    staffPos: 4,
  },
  {
    name: 'G#',
    isHalf: true,
    staffPos: 4,
  },
  {
    name: 'A',
    staffPos: 5,
  },
  {
    name: 'A#',
    isHalf: true,
    staffPos: 5,
  },
  {
    name: 'B',
    staffPos: 6,
  },
]

// resolves any +/- integer to a note relative middle C at 0
const getNote = toneNumber => notes[(12 + (toneNumber % 12)) % 12];

// determines the staff position relative to middle C of a tone based on its corresponding note
function tonePos(toneNumber) {
  const note = getNote(toneNumber);
  // tone 12 (high c) should be position 7
  // for every octave above 11, add 7 to note position?
  return note.staffPos + (Math.floor(toneNumber / 12) * 7);
}

// create web audio api context
var audioCtx = new (window.AudioContext || window.webkitAudioContext)();

function playTone(frequency, duration) {
  // create Oscillator node
  var oscillator = audioCtx.createOscillator();

  oscillator.frequency.value = frequency; // value in hertz
  oscillator.connect(audioCtx.destination);
  oscillator.start();

  setTimeout(
    function () {
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