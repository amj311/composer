<script setup>
import { ref } from 'vue'

defineProps({
  note: Object,
  x: Number,
  y: Number,
})

const count = ref(0)
</script>

<template>
  <!--  single note no stem -->
  <svg :x="x" :y="y" width=14 height=12 stroke="black">
    <g style="transform-origin: center;" :style="{ transform: note.type < 1 ? 'rotate(-30deg) scale(0.9)' : ''}">
      <path stroke="black" stroke-width="0"
        d="M 6.5,0 a 7,6 0 1,0 1,0 z     M 6.5,3 a 6,3 0 1,0 1,0 z" :fill-rule="note.type < 1/2 ? null : 'evenodd'" fill="black" />
    </g>
<!--         <line x1=1 y1=9 x2=1 y2=22 stroke-width=1.5 /> -->
  </svg>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
