<template>
    <v-main class="card" :class="{ 'disable': isDisabled }" :style="{
        height: `${((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16)}px`,
        width: `${((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 / 4}px`,
    }">
        <div :class="['card__inner', { 'is-flipped': isFlipped }]" @click="toggleCard()">
            <div class="card__face card__face--front">
                <div class="card__content">

                </div>
            </div>
            <div class="card__face card__face--back">
                <div class="card__content"
                    :style="{ backgroundImage: `url(${require(`@/assets/images/${card.value}.png`)})` }">
                </div>
            </div>
        </div>
    </v-main>
</template>

<script>
export default {
    name: 'Card-Item',
    props: {
        card: {
            required: true,
        },
        cardContext: {
            required: true,
        },
    },
    data: () => ({
        isFlipped: false,
        isDisabled: false,
    }),
    methods: {
        toggleCard() {
            this.isFlipped = !this.isFlipped;
            if (this.isFlipped) {
                this.$emit('on-flip', this.card);
            }
        },
        onFlipBackCard() {
            this.isFlipped = false;
        },
        onDisableCard() {
            this.isDisabled = true;
        },
    },
}
</script>

<style lang="css" scoped>
.disable {
    pointer-events: none;
}

.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
    width: 90px;
    height: 120px;
}

.card__inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
    border-radius: 1em;
    background-color: var(--light);
}

.card__inner.is-flipped {
    transform: rotateY(-180deg);
}

.card__face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.1);
}

.card__face--front .card__content {
    background: url("../assets/images/icon_back.png") no-repeat center center;
    background-size: contain;
    height: 100%;
    width: 100%;
}

.card__face--back .card__content {
    background-position: center center;
    background-size: contain;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
}

.card__face--back {
    background-color: var(--light);
    transform: rotateY(-180deg);
}
</style>