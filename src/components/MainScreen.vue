<template>
    <v-main class="screen-main">
        <h1>Pokemon Game</h1>
        <h3>Select mode to play</h3>
        <div class="actions">
            <v-btn class="btn-primary ma-4" @click="onStart(16)" color="primary" size="x-large">
                <span>4x4</span>
                <span>Easy</span>
            </v-btn>
            <v-btn class="btn-primary ma-4" color="primary" @click="onStart(36)" size="x-large">
                <span>6x6</span>
                <span>Medium</span>
            </v-btn>
            <v-btn class="btn-primary ma-4" color="primary" @click="onStart(64)" size="x-large">
                <span>8x8</span>
                <span>Hard</span>
            </v-btn>
            <v-btn class="btn-primary ma-4" color="primary" @click="onStart(100)" size="x-large">
                <span>10x10</span>
                <span>Very Hard</span>
            </v-btn>
        </div>
    </v-main>
</template>

<script>


export default {
    name: 'MainScreen',
    methods: {
        onStart(totalOfBlocks) {
            this.$emit('on-start', {
                totalOfBlocks
            });
        }
    },

    data: () => ({
        //
    }),
}
</script>
<style scoped>
.screen-main {
    width: 100%;
    height: 100vh;
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    background-color: var(--dark);
    color: var(--light);
}

h1 {
    font-size: 5rem;
    margin-bottom: 1rem;
}

h3 {
    font-size: 2rem;
    margin-bottom: 1rem;
}
</style>