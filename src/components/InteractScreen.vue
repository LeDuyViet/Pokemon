<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{ width: `${(((((768 - 16 * 4) / Math.sqrt(cardContext.length)) - 16) * 3 / 4) + 16) * Math.sqrt(cardContext.length)}px` }"
    >
      <card-flip
        v-for="(Card, index) in cardContext"
        :key="index"
        :urlBackFaceUrl="`images/${Card}.png`"
        :card="{ index, value: Card }"
        @onToggleFlipCard="checkRules($event)"
        :ref="`card-${index}`"
        :rules="rules"
        :cardContext="cardContext"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";
export default {
  components: {
    CardFlip,
  },
  props: {
    cardContext: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      rules: [],
      countCard: 0,
    };
  },
  methods: {
    checkRules(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);

      // check dung
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value &&
        this.rules[0].index !== this.rules[1].index
      ) {
        this.$refs[`card-${this.rules[0].index}`].turnOnDisabledMode();
        this.$refs[`card-${this.rules[1].index}`].turnOnDisabledMode();
        this.countCard++;
        this.rules = [];
        console.log(this.countCard);

        if (this.countCard == this.cardContext.length / 2) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 5000);
        }

        // let disabledEl = document.getElementsByClassName('disabled');
        // if (disabledEl)
        //   console.log("end game", disabledEl);

        // if (disabledEl && disabledEl.length === this.cardContext.length - 2) {
        //   console.log("end game", disabledEl);
        // }
      }
      // check sai
      else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1]
      ) {
        // dong the
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`].onFlipBackCard();
          // reset rule
          this.rules = [];
        }, 1000);
      } else {
        // setTimeout(() => {
        //   this.$refs[`card-${this.rules[0].index}`].onFlipBackCard();
        //   this.$refs[`card-${this.rules[1].index}`].onFlipBackCard();
        //   // reset rule
        //   this.rules = [];
        // }, 1000);
        return false;
      }
    },
  },
};
</script>

<style scoped>
.screen {
  width: 100%;
  height: 100%;
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
}
</style>
