<template>
    <div class="screen">
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
            if (this.rules.length == 2) return false;

            this.rules.push(card);

            if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
                console.log("right...");
                // add class 'disabled' to component card
                const cardOne1 = this.$refs[`card-${this.rules[0].index}`];
                cardOne1[0].onEnabledDisableMode();
                const cardTwo1 = this.$refs[`card-${this.rules[1].index}`];
                cardTwo1[0].onEnabledDisableMode();

                // reset rules []
                this.rules = [];

                const disabledElements = document.querySelectorAll(".screen .card.disabled");

                if (disabledElements && disabledElements.length === this.cardsContext.length - 2) {
                    setTimeout(() => {
                        this.$emit("onFinish");
                    }, 920);
                }
            }
            else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
                console.log("wrong...");
                // close two card
                setTimeout(() => {
                    const cardOne = this.$refs[`card-${this.rules[0].index}`];
                    cardOne[0].onFlipBackCard();
                    const cardTwo = this.$refs[`card-${this.rules[1].index}`];
                    cardTwo[0].onFlipBackCard();

                    // reset rules []
                    this.rules = [];
                }, 800);
            }
            else return false;
        }
    },
}
</script>