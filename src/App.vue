<template>
  <img alt="Vue logo" src="./assets/logo.png">  
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen v-if="statusMatch === 'match'" 
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
    />   
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch='default'"/>
  <footer-copyright />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import FooterCopyright from "./components/FooterCopyright.vue";

import {shuffled} from "./utils/array";

export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    FooterCopyright,
  }, 
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startAt: null,
      },
      statusMatch: "default",
      timer: 0,
    }
  },
 
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from({length: this.settings.totalOfBlocks / 2}, 
      (_, i) => i + 1
        );
      const secondCards = [...firstCards];
      const card = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(card))));
      this.settings.startAt = new Date().getTime();

      console.log("This is config", this.settings.cardsContext);
      this.statusMatch = "match";
    },
    onGetResult() {
      // Get timer
      this.timer = new Date().getTime() - this.settings.startAt;
      // Switch to result compponent
      this.statusMatch = "result";

    }
  }
}
</script>
