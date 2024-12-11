<template>
  <div class="w-full" @wheel="getTop()" @touchmove="getTop()">
    <Map :state="state" class="fixed z-0 w-full left-0 top-0" />
    <div v-if="textBlocks" class="relative z-1 w-full flex flex-col px-10" :style="wrapperStyle">
      <FlyingTextBlock
        v-for="(block, key) in textBlocks"
        :key="key"
        :id="key"
        :text="block"
        :margin-bottom="marginBottomCalculated"
        :first="key === 'one'"
      >
        {{ key }}
      </FlyingTextBlock>
    </div>
  </div>
</template>

<script setup lang="ts">
import Map from "./components/Map.vue";
import FlyingTextBlock from "./components/FlyingTextBlock.vue";
import { ref, onMounted, onBeforeUnmount } from "vue";

type TextBlockObject = {
  text: string;
};

type TextBlockDictionary = {
  [key: string]: TextBlockObject;
};

const height = window.innerHeight;
const gapBlocksFormatted = `gap:${height}px`;
const marginBottomCalculated = `margin-bottom:${height + 100}px`;
const paddingTop = `padding-top:${height / 8}px`;
const wrapperStyle = gapBlocksFormatted + ";" + marginBottomCalculated + ";" + paddingTop;

let state = ref(1);
let textBlocks = ref<TextBlockDictionary>();

let one: HTMLElement | null;
let two: HTMLElement | null;
let twoA: HTMLElement | null;
let twoB: HTMLElement | null;
let three: HTMLElement | null;
let four: HTMLElement | null;
let five: HTMLElement | null;
let six: HTMLElement | null;

// get html block elements
function getHtmlElements() {
  one = document.getElementById("one");
  two = document.getElementById("two");
  twoA = document.getElementById("two-a");
  twoB = document.getElementById("two-b");
  three = document.getElementById("three");
  four = document.getElementById("four");
  five = document.getElementById("five");
  six = document.getElementById("six");
}

function getTop() {
  // state handler
  if (one && two && one.getBoundingClientRect().top > 0 && two.getBoundingClientRect().top > height) state.value = 1;
  if (one && one.getBoundingClientRect().top < 0) state.value = 2;
  if (two && two.getBoundingClientRect().bottom < height) state.value = 2;
  if (twoA && twoA.getBoundingClientRect().bottom < height) state.value = 2.5;
  if (twoB && twoB.getBoundingClientRect().bottom < height) state.value = 2.6;
  if (three && three.getBoundingClientRect().bottom < height) state.value = 3;
  if (four && four.getBoundingClientRect().bottom < height) state.value = 4;
  if (five && five.getBoundingClientRect().bottom < height) state.value = 5;
  if (six && six.getBoundingClientRect().bottom < height) state.value = 6;
}

async function fetchBlockTexts() {
  try {
    const response = await fetch("/blockTexts.json");
    if (!response.ok) {
      throw new Error("Network response was not ok");
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error fetching blockTexts:", error);
  }
}

function handleBeforeUnload() {
  window.scrollTo(0, 0);
}

onMounted(async () => {
  textBlocks.value = await fetchBlockTexts();
  setTimeout(() => {
    getHtmlElements();
  }, 1000);
  window.addEventListener("beforeunload", handleBeforeUnload);
});

onBeforeUnmount(() => {
  window.removeEventListener("beforeunload", handleBeforeUnload);
});
</script>
