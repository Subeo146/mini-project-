<template>
  <div class="screen">
    <h1>Play Game here ....</h1>
    <card-flip
      v-for="(card, index) in cardsContext"
      :key="index"
      :ref="`card-${index}`"
      :imgBackFaceUrl="`images/${card}.png`"
      :card="{ index, value: card }"
      @onFlip="checkRule($event)"
    />
  </div>
</template>
<script>
import CardFlip from "./Card.vue";
export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      rules: [],
    };
  },
  components: {
    CardFlip,
  },
  methods: {
    checkRule(card) {
      //console.log(card);

      if (this.rules.length === 2) return false;
      this.rules.push(card);
      //ktra chi 2 phan tu
      //console.log(this.rules);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("alright...");
        // dissable card
        this.$refs[`card-${this.rules[0].index}`].onDisableBackCard();
        this.$refs[`card-${this.rules[1].index}`].onDisableBackCard();

        //refresh
        this.rules = [];

        //check end game

        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        //console.log(disabledElements);
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        )
          setTimeout(() => {
            this.$emit("onFinish");
          }, 970);
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("wrong....");
        //close two card
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`].onFlipBackCard();
          //refresh
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>