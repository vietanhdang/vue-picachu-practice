<template>
    <div class="app">
        <h2 v-if="isPlaying">
            <v-btn @click="pauseAudio" color="primary">
                <v-icon>mdi-pause</v-icon>
            </v-btn>
        </h2>
        <h2 v-else>
            <v-btn @click="playAudio" color="primary">
                <v-icon>mdi-play</v-icon>
            </v-btn>
        </h2>
        <h1>This game create by Vue.js - <a href="https://www.facebook.com/anhdv47/">Việt Anh</a></h1>
    </div>
</template>

<script>
export default {
    name: 'AudioAutoPlay',
    data() {
        return {
            isPlaying: false,
            audio: new Audio(require('../assets/audio/1.mp3'))
        };
    },
    methods: {
        playAudio() {
            this.isPlaying = true;
            this.audio.currentTime = 7;
            this.audio.play();
        },
        pauseAudio() {
            this.isPlaying = false;
            this.audio.pause();
        },
    },

}
</script>
<style scoped>
.app {
    z-index: 2;
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    color: green;
}

h1 a {
    color: green;
}
</style>