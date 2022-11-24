<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'
import { ref } from "vue";



const colors = [
  'red', 'blue', 'green', 'yellow'
];
const step = ref(null);
const nbStroke = ref(0);

// const simonList = [
//   'red', 'red', 'blue', 'yellow', 'blue', 'green', 'red', 'green', 'yellow', 'blue', 'green', 'red', 'yellow', 'blue', 'green', 'red'
// ];
const simonList = ['red'];


const tellColor = (color) => {
  if ('speechSynthesis' in window) {
    var synthesis = window.speechSynthesis;

    // Get the first `en` language voice in the list
    var voice = synthesis.getVoices().filter(function (voice) {
      return voice.lang === 'en';
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

// // const sayColor = () => {
//   if ('webkitSpeechRecognition' in window) {
//     // start_button.style.display = 'inline-block';
//     recognition = new webkitSpeechRecognition();
//     recognition.continuous = true;
//     recognition.interimResults = true;
//     recognition.onstart = () => {
//        recognizing = true;
//        showInfo('info_speak_now');
//        start_img.src = './src/images/mic-animate.gif';
//     };
//     recognition.onresult = (event) => {
//        let interim_transcript = '';
//        for (let i = event.resultIndex; i < event.results.length; ++i) {
//           if (event.results[i].isFinal) {
//              final_transcript += event.results[i][0].transcript;
//           } else {
//              interim_transcript += event.results[i][0].transcript;
//           }
//        }
//        final_transcript = capitalize(final_transcript);
//        title.innerHTML = linebreak(final_transcript);
//        interim_span.innerHTML = linebreak(interim_transcript);
//      };
//     // recognition.onerror = (event) => {
//     // };
//     // recognition.onend = () => {
//     // };
// }else{
//   console.log('nononon');
// }
const sendNotification = () => {
  Notification.requestPermission().then((result) => {
    if (result === "granted") {
      const notifTitle = "Simon game";
      const notifBody = "Suuuu";
      const options = {
        body: notifBody,
      };
      new Notification(notifTitle, options);
    }
  });
};

sendNotification();


const startGame = () => {
  step.value = 0;
  nbStroke.value = 0;
  tellColor(simonList[step.value]);

}

const handleClick = (color) => {
  
  console.log(simonList.length);
  console.log(step.value);
  if (color == simonList[nbStroke.value]) {
    if (nbStroke.value === step.value) {
      if (step.value == simonList.length) {
        tellColor('gagnééé')
        restart();
      }else{
        step.value++;
        nbStroke.value = 0;
        simonList.slice(0, step.value + 1).forEach(element => {
          tellColor(element);
        });
      }
      
      
    }else{
      nbStroke.value ++;
    }
  } else {
    tellColor('perdu')
    restart();
  }

}

const restart = () => {
  step.value = null;
  nbStroke.value = 0;
}



</script>

<template>
  <button v-if="step == null" @click="startGame">start</button>

  <div>
    <p>Etape</p>
    <p v-if="step != null">{{ step }}</p>
  </div>


  <div v-if="step >= 0 && step !=null">
    <button v-for="(color, index) in colors" :key="index"
    :class="{ red: (color == 'red'), yellow: (color == 'yellow'), blue: (color == 'blue'), green: (color == 'green'), }"
    style="color: white;border-color: white;" @click="handleClick(color)">{{ color }}</button>
  </div>
 

</template>

<style scoped>
.red {
  background-color: red;
}

.blue {
  background-color: blue;
}

.green {
  background-color: green;
}

.yellow {
  background-color: yellow;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
