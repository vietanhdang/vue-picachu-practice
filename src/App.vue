<template>
  <v-app>
    <MainScreenVue v-if="status === 'main'" @on-start="onHandleStart" />
    <InteractScreen v-if="status === 'interact'" :cards="setting" @on-back="onHandleBack"
      @on-restart="onHandleRestart" />
    <AudioAutoPlay ref="audio" />
  </v-app>
</template>

<script>


import MainScreenVue from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen.vue';
import AudioAutoPlay from './components/AudioAutoPlay.vue';
export default {
  name: 'App',
  components: {
    MainScreenVue,
    InteractScreen,
    AudioAutoPlay,
  },
  data: () => ({
    setting: {
      totalOfBlocks: 0,
      cardContent: [],
      endTime: 0,
    },
    status: 'main',
  }),
  methods: {
    onHandleStart(data) {
      this.setting.totalOfBlocks = data.totalOfBlocks;
      const firstCard = Array.from({ length: this.setting.totalOfBlocks / 2 }, (_, k) => k + 1);
      const secondCard = [...firstCard];
      const card = [...firstCard, ...secondCard];
      card.sort(() => Math.random() - 0.5);
      this.setting.cardContent = card;
      this.setting.endTime = data.totalOfBlocks * 10;
      this.status = 'interact';
      this.isPlaying = this.$refs.audio.isPlaying;
      if (!this.$refs.audio.isPlaying) {
        this.$refs.audio.playAudio();
      }
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
