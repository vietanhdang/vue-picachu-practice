<template>
  <v-app>
    <MainScreenVue v-if="status === 'main'" @on-start="onHandleStart" />
    <InteractScreen v-if="status === 'interact'" :cards="setting" @on-back="onHandleBack"
      @on-restart="onHandleRestart" />
    <h1>This game create by Vue.js - <a href="https://www.facebook.com/anhdv47/">Việt Anh</a></h1>

  </v-app>
</template>

<script>


import MainScreenVue from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen.vue';
export default {
  name: 'App',
  components: {
    MainScreenVue,
    InteractScreen,
  },
  data: () => ({
    setting: {
      totalOfBlocks: 0,
      cardContent: [],
      endTime: 0,
    },
    status: 'main',
  }),
  watch: {
    timerCount() {
      console.log(this.timerCount);
    }
  },
  methods: {
    onHandleStart(data) {
      this.setting.totalOfBlocks = data.totalOfBlocks;
      const firstCard = Array.from({ length: this.setting.totalOfBlocks / 2 }, (_, k) => k + 1);
      const secondCard = [...firstCard];
      const card = [...firstCard, ...secondCard];
      card.sort(() => Math.random() - 0.5);
      this.setting.cardContent = card;
      this.setting.endTime = data.totalOfBlocks * 20;
      this.status = 'interact';
    },
    onHandleBack(data) {
      this.status = data;
    },
    onHandleRestart(data) {
      this.status = 'main';
      setTimeout(() => {
        this.onHandleStart(data);
      }, 0);
    },
  }
}
</script>
<style scoped>
h1 {
  z-index: 1;
  color: var(--light);
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}

h1 a {
  color: cadetblue
}
</style>