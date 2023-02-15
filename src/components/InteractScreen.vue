<template>
    <div class="screen">
        <div class="card-grid" :style="{
            ...cssVars,
            'width': cardsContext.length > 64 ? '1200px' : '900px'
        }">
            <card-flip v-for="(card, index) in cardsContext" 
                :key="index" 
                :card="{ index, value: card }"
                :ref="`card${index}`"
                :rules="rules"
                :imgBackFaceUrl="`images/${card}.png`" 
                @onFlip="checkRule($event)"/>
        </div>
    </div>
</template>

<script>
import { getSqrt } from '@/helper/helper';
import CardFlip from './Card.vue';

export default {
    props: {
        cardsContext: {
            type: Array,
            default: () => []
        }
    },
    components: {
        CardFlip
    },
    data() {
        return {
            rules: []
        }
    },
    computed: {
        cssVars() {
            return {
                '--num-block': getSqrt(this.cardsContext.length)
            }
        }
    },
    methods: {
        checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      if (
        this.rules.length >= 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("Right...");
        this.$refs[`card${this.rules[0].index}`][0].onDisabledFlip();
        this.$refs[`card${this.rules[1].index}`][0].onDisabledFlip();
        this.rules = [];

        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        )
          setTimeout(() => {
            this.$emit("onFinish")
          }, 920)
      } else if (
        this.rules.length >= 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("wrong!");
        setTimeout(() => {
          this.$refs[`card${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);
      } else return false;
    },
    }
}
</script>

<style scoped>
.card-grid {
    display: grid;
    grid-template-columns: repeat(var(--num-block), 1fr);
    grid-gap: 30px;
    align-items: center;
    justify-content: center;
    margin: 30px auto;
}
</style>