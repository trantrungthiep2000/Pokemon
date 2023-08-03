<template>
  <main-screen 
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  ></main-screen>
  <interact-screen 
    v-if="statusMatch === 'match'" 
    :cardsContext="settings.cardsContext"
  ></interact-screen>
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import InteractScreen from "./components/InteractScreen.vue"
import { shuffled } from "./utils/array.js"

export default {
  name: 'App',
  components: {
    MainScreen,
    InteractScreen
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
    }
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from({ length: this.settings.totalOfBlocks / 2 }, (_, i) => i+1);
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
      this.settings.startedAt = new Date().getTime();
      // data ready
      this.statusMatch = "match";
    }
  }
}
</script>