<template>
    <div class="card">
        <div class="card_inner" :class="{ 'is_flipped': isFlipped }" @click="onTonggleFlipCard">
            <div class="card_face card_face_front">
                <div class="card_content"></div>
            </div>
            <div class="card_face card_face_back">
                <div class="card_content" :style="{ backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,}"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    props: {
        imgBackFaceUrl: {
            type: String,
            required: true,
        },
        card: {
            type: [String, Number, Array, Object],
        },
    },
    data() {
        return {
            isFlipped: false
        }
    },
    methods: {
        onTonggleFlipCard() {
            this.isFlipped = !this.isFlipped;

            if (this.isFlipped) this.$emit("onFlip", this.card);
        },

        onFlipBackCard() {
            this.isFlipped = false;
        },
    },
}
</script>

<style lang="css">
.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
    width: 90px;
    height: 120px;
}

.card_inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
}

.card_inner.is_flipped {
    transform: rotateY(-180deg);
}

.card_face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}
.card_face_front .card_content{
    background: url(../assets/images/icon_back.png) no-repeat center center;
    background-size: 40px 40px;
    height: 100%;
    width: 100%;
}

.card_face_back {
    background-color: var(--light);
    transform: rotateY(-180deg);
}

.card_face_back .card_content{
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
    height: 100%;
    widows: 100%;
}
</style>