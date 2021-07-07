<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandlerbeforeStart($event)"
  />
  <interact-screen v-if="statusMatch === 'match'"
  :cardsContext="settings.cardsContext" 
  @onFinish="onGetResult"
  />
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch='default'"  />
  <coppy-right-screen />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue"
import CoppyRightScreen from "./components/CopyRightScreen.vue"
import { shuffled } from "./utils/array"
export default {
  name: "App",
  components: { MainScreen, InteractScreen ,ResultScreen,CoppyRightScreen },
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
  methods: {
    onHandlerbeforeStart(config) {
      console.log("Running onHandler before start, ", config);
      this.settings.totalOfBlocks = config.totalOfBlock;
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards]; //clone
      const cards = [...firstCards, ...secondCards] //mix 2 array
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));
      this.settings.startedAt = new Date().getTime(); 
      this.statusMatch = "match";
    },
    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      console.log(this.timer)
      this.statusMatch = 'result';

    }
  },
};
</script>

<style>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
}
</style>
