<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(768 - 16 * 4) / Math.sqrt(cardContext.length) - 16}px`,
      width: `${
        (((768 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((768 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              1.5
            }px ${
              (((768 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              1.5
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + urlBackFaceUrl)})`,
            backgroundSize: `${
              (((768 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              1.5
            }px ${
              (((768 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              1.5
            }px`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    urlBackFaceUrl: {
      type: String,
      required: true,
    },
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
    card: {
      type: [String, Object, Array, Number],
      required: true,
    },
    rules: {
      type: Array,
    },
  },
  data() {
    return {
      isFlipped: false,
      isDisabled: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;
      if (this.rules.length === 2) return false;
      this.isFlipped = !this.isFlipped;
      this.$emit("onToggleFlipCard", this.card);
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    turnOnDisabledMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
  perspective: 200px;
}
.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.is-flipped {
  transform: rotateY(-180deg);
}
.card__face {
  width: 100%;
  height: 100%;
  position: absolute;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
  border-radius: 1rem;
  overflow: hidden;
  backface-visibility: hidden;
}
.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  /* background-size: contain; */
  width: 100%;
  height: 100%;
}
.card__face--back .card__content {
  background-position: center center;
  background-repeat: no-repeat;
  background-color: var(--light);

  /* background-size: contain; */
  height: 100%;
  width: 100%;
}
.card__face--back {
  transform: rotateY(-180deg);
}
.card.disabled .card__inner {
  cursor: default;
}
</style>
