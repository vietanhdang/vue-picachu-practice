<template>
    <v-main class="screen">
        <div class="screen__inner" :style="{
            width: `${((((960 - 16 * 4) / Math.sqrt(cards.cardContent.length) - 16) * 3) / 4 +
                16) *
                Math.sqrt(cards.cardContent.length)
                }px`,
            height: `100vh`,
        }">
            <v-card class="timer">
                <v-card-title>
                    <h1>Time Left : {{ timerCount }}s</h1>
                </v-card-title>
                <v-card-actions class="justify-center">
                    <v-btn flat color="error" @click="handleGame('back')" background-color="red" outlined rounded>
                        Back to Main Screen
                    </v-btn>
                </v-card-actions>
            </v-card>

            <div :class="{ 'disable': timerCount == 0 }">
                <Card v-for="(card, index) in cards.cardContent" :key="index" :card="{ index, value: card }"
                    :cardContext="cards.cardContent" :ref="`cardIndex-${index}`" @on-flip="onHandleFlip"
                    :class="{ 'disable': isDisabled }" />
            </div>
        </div>
        <v-dialog v-model="dialog" persistent max-width="290">
            <v-card>
                <v-card-title class="text-h5">
                    {{ message }}
                </v-card-title>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="green darken-1" text @click="handleGame('back')">
                        Back to Main Screen
                    </v-btn>
                    <v-btn color="green darken-1" text @click="handleGame('restart')">
                        Restart
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-main>
</template>

<script>

import Card from './Card.vue';
// import { ref } from 'vue';
export default {
    name: 'InteractScreen',
    props: {
        cards: {
            type: Object,
            required: true,
        },
    },
    components: {
        Card,
    },

    data: () => ({
        rules: [],
        isDisabled: false,
        timerCount: 0,
        dialog: false,
        done: [],
        message: '',
    }),
    mounted() {
        this.timerCount = this.cards.endTime;
    },
    watch: {
        timerCount: {
            handler(value) {
                if (value > 0) {
                    setTimeout(() => {
                        this.timerCount--;
                        if (this.timerCount == 0) {
                            this.message = 'Time Up';
                            this.dialog = true;
                        }
                        if (this.done.length == this.cards.cardContent.length) {
                            this.message = 'You Win';
                            this.dialog = true;
                        }
                    }, 1000);
                }
            },
            immediate: true // This ensures the watcher is triggered upon creation
        }
    },
    methods: {
        onHandleFlip(card) {
            if (this.rules.length == 2) return false;
            this.rules.push(card);
            if (this.rules.length == 2) {
                this.isDisabled = true;
                if (this.rules[0].value == this.rules[1].value) {
                    this.$refs[`cardIndex-${this.rules[0].index}`][0].onDisableCard();
                    this.$refs[`cardIndex-${this.rules[1].index}`][0].onDisableCard();
                    this.done.push(this.rules[0].index);
                    this.done.push(this.rules[1].index);
                    this.isDisabled = false;
                    this.rules = [];
                } else {
                    setTimeout(() => {
                        this.$refs[`cardIndex-${this.rules[0].index}`][0].onFlipBackCard()
                        this.$refs[`cardIndex-${this.rules[1].index}`][0].onFlipBackCard()
                        this.isDisabled = false;
                        this.rules = [];
                    }, 1000);
                }
            }
        },
        handleGame(action) {
            if (action == 'back') {
                this.$emit('on-back', 'main');
            } else if (action == 'restart') {
                this.$emit('on-restart', {
                    totalOfBlocks: this.cards.cardContent.length,
                });
            }
        },
    },
}
</script>
<style scoped lang="css">
.timer {
    position: absolute;
    top: 0;
    left: 0;
    width: 300px;
    text-align: center;
}

.disable {
    pointer-events: none;
}

.actions {
    /* display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%; */
    position: absolute;
    top: 45px;
    left: 0;
}

.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    z-index: 2;
    background: url('../assets/images/41380.jpg') no-repeat center center fixed;
    background-size: cover;
    color: var(--light);
}

.screen__inner {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    margin: 2rem auto;
}
</style>