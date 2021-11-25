<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <play-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onResult"
  />
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onPlayAgain= "statusMatch = 'default'" />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import PlayScreen from "./components/PlayScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array.js";
export default {
  name: "App",
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },

  components: {
    MainScreen,
    PlayScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running handle before start here....", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCard = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      //console.log(firstCard);

      const secondCard = [...firstCard];

      const cards = [...firstCard, ...secondCard];
      //console.log(cards);

      this.settings.cardsContext = shuffled(shuffled(shuffled(cards)));
      //console.log(this.settings.cardsContext);

      this.settings.startedAt = new Date().getTime();

      //data ready
      this.statusMatch = "match";
    },
    onResult() {
      // thoi gian hoan thanh
      this.timer = new Date().getTime() - this.settings.startedAt;
      // chuyen man hinh result
      this.statusMatch="result";
    },
  },
};
</script>

<style>
</style>