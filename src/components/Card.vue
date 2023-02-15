<template>
    <div class="card" :class="{
        disabled: isDisabled
    }">
        <div class="card__inner" 
        :class="{ 'is-flipped' : isFlipped }"
        @click="onToggleFlipCard($event)">
            <div class="card__face card__face--front">
                <div class="card__content"></div>
            </div>
            <div class="card__face card__face--back">
                <div class="card__content" 
                :style="{
                    backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`
                }"
                ></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CardFlip',
    props: {
        cardsContext: {
            type: Array,
            default: () => []
        },
        imgBackFaceUrl: {
            type: String,
            required: true
        },
        card: {
            type: [String, Number, Array, Object]
        },
        rules: {
            type: Array,
        },
    },
    data() {
        return {
            isFlipped: false,
            isDisabled: false
        }
    },
    methods: {
        onToggleFlipCard() {
            if (this.rules.length >= 2) return
            if (this.isDisabled) return false
            this.isFlipped = !this.isFlipped
            if (this.isFlipped) this.$emit('onFlip', this.card)
        },
        onFlipBackCard() {
            this.isFlipped = false
        }, 
        onDisabledFlip() {
            this.isDisabled = true
        }
    }
}
</script>

<style lang="css" scoped>
    .card {
        width: 100%;
        aspect-ratio: 1/1.1;
    }
    .card__inner {
        width: 100%;
        height: 100%;
        transition: transform .5s linear;
        transform-style: preserve-3d;
        cursor: pointer;
        position: relative;
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
        padding: 1rem;
        box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
    }

    .card__face--front .card__content{
        background: url('../assets/images/icon_back.png') no-repeat center center;
        height: 100%;
        width: 100%;
        background-size: 50px 50px;
    }

    .card__face--back .card__content {
        height: 100%;
        width: 100%;
        background-size: 50px 50px;
        background-position: center;
        background-repeat: no-repeat;
    }

    .card__face--back {
        background-color: var(--light);
        transform: rotateY(-180deg);
    }

    .card.disabled .card__inner{
        cursor: default;
    }
</style>