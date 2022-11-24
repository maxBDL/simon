<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    color
});

const tellColor = (color) => {
  if ('speechSynthesis' in window) {
    var synthesis = window.speechSynthesis;

    // Get the first `en` language voice in the list
    var voice = synthesis.getVoices().filter(function (voice) {
      return voice.lang === 'fr';
    })[0];

    // Create an utterance object
    var utterance = new SpeechSynthesisUtterance(color);

    // Set utterance properties
    utterance.voice = voice;
    utterance.pitch = 1.5;
    utterance.rate = 1.25;
    utterance.volume = 0.8;

    // Speak the utterance
    synthesis.speak(utterance);
  } else {
    console.log('Text-to-speech not supported.');
  }
}
</script>

<template>
    <div>
        <button  :key="index" style="color: white;border-color: white;" @click="tellColor(color)" >{{color}}</button>
    </div>
</template>