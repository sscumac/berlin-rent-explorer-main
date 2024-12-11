<template>
  <div id="wrapper relative ">
    <div class="w-full relative max-w-screen-xl mx-auto">
      <div
        class="absolute border-4 border-gray-800 m-2 left-2 xl:right-auto bg-white z-10 p-1 sm:p-2 sm:text-xl opacity-90"
      >
        {{ selectedYear }}
      </div>
    </div>
    <div
      id="tooltip"
      class="absolute transition-opacity border-2 border-gray-800 right-4 top-2 md:right-auto bg-white p-2 z-[1000] opacity-0 pointer-events-none"
    >
      <p>{{ selected?.PROGNOSERA }}</p>
      <p>{{ selected?.ANGEBOTSMIETEN.toFixed(2) }} € / m²</p>
    </div>
    <div class="relative">
      <div id="main-map" class="w-full relative" :style="mapHeightCalculated">
        <div class="h-full w-full absolute bg-white z-[1]"></div>
        <div id="border-map" :class="[mapIsMoving ? 'invisible' : 'visible']" class="w-full z-1 relative h-full"></div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
// define all props
defineProps<{
  selectedYear: number;
  selected:
    | {
        PROGNOSERA: string;
        ANGEBOTSMIETEN: number;
      }
    | undefined;
  mapHeightCalculated: string;
  mapIsMoving: boolean;
  state: number | undefined;
}>();
</script>
