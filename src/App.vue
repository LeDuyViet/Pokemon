<template>
  <main-screen
    @onStart="onHanderBeforeStart($event)"
    v-if="statusMatch === 'default'"
  />
  <interact-screen v-if="statusMatch === 'match'" :cardContext="settings.cardContext" @onFinish="onGetResult" />
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="this.statusMatch='default'" />
  <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array.js";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight,
  },
  data() {
    return {
      settings: {
        totalCardCount: 0,
        cardContext: [],
        startAt: null,
      },
      statusMatch: "default",
      timer: null,
    };
  },
  methods: {
    onHanderBeforeStart(config) {
      // tao ra mang phan tu
      this.settings.totalCardCount = config.totalCardCount;

      const firstCards = Array.from(
        { length: this.settings.totalCardCount / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];

      // tron mang phan tu
      this.settings.cardContext = shuffled(shuffled(shuffled(cards)));

      // switch to match
      this.settings.startAt = new Date().getTime();
      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startAt;
      this.statusMatch = "result"
    }
  },
};
</script>

<style></style>
