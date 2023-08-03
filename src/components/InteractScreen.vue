<template>
    <div class="scre">
        <h1>Interact Component here...</h1>
        <card-flip 
            v-for="(card, index) in cardsContext" 
            :key="index" 
            :ref="`card-${index}`"
            :imgBackFaceUrl="`images/${card}.png`"
            :card="{ index, value: card }"
            @onFlip="checkRule($event)"
        ></card-flip>
    </div>
</template>

<script>
import CardFlip from "./CardFlip.vue"

export default{
    name: "InteractScreen",
    components: {
        CardFlip,
    },
    props: {
        cardsContext: {
            type: Array,
            default: function(){
                return [];
            },
        },
    },
    data() {
        return {
            rules: [],
        }
    },
    methods: {
        checkRule(card) {
            if (this.rules.length === 2) return false;

            this.rules.push(card);

            if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
                console.log("right...");
            }
            else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
                console.log("wrong...");
                // close two card
                setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`].onFlipBackCard();
                }, 800);

                // reset rules []
                this.rules = [];
            }
            else return false;
        }
    },
}
</script>