<template>
  <div class="text-center max-w-xl">
    <h2 class="text-3xl font-semibold">Summary</h2>
    <div class="my-4">
      <p class="mb-2 text-xl font-semibold">{{ "Name: " + name }}</p>
      <p class="mb-2">{{ "Age: " + age }}</p>
      <p class="mb-2">{{ "Where do you live: " + region }}</p>
      <p class="mb-2">{{ "Package: " + pack }}</p>
      <p class="mb-2">{{ "Premium: " + amount + currencyCode }}</p>
    </div>
    <div>
      <Button @click="handleGoBack" text="Back" :type="'outlined'" class="w-28 h-12" />
      <Button @click="handleBuy" text="Buy" class="w-28 h-12"/>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Button from "~/ui-core/Button.vue";
import { CURRENCY_CODE, PLANS, REGIONS } from "~/utils/constants";

export default defineComponent({
  name: "Summary",
  components: {
    Button,
  },
  props: {
    age: {
      required: true,
      type: Number,
    },
    name: {
      required: true,
      type: String,
    },
    pack: {
      required: true,
      type: String,
    },
    region: {
      required: true,
      type: String,
    },
    resetValues: {
      required: true,
      type: Function,
    },
  },
  methods: {
    handleRedirection(page: string) {
      if (this.$router) {
        this.$router.push({
          query: {
            page,
          },
        });
      }
    },
    handleGoBack() {
      this.handleRedirection("2");
    },
    handleBuy() {
      this.handleRedirection("1");
      this.resetValues();
    },
  },
  computed: {
    rate() {
      switch (this.region) {
        case REGIONS.HONG_KONG:
          return 1;
        case REGIONS.USA:
          return 2;
        case REGIONS.AUSTRALIA:
          return 3;
        default:
          return 1;
      }
    },
    currencyCode() {
      switch (this.region) {
        case REGIONS.USA:
          return CURRENCY_CODE.USA;
        case REGIONS.AUSTRALIA:
          return CURRENCY_CODE.AUSTRALIA;
        default:
          return CURRENCY_CODE.HONG_KONG;
      }
    },
    amount() {
      const value: number = this.age * 10 * this.rate;
      switch (this.pack) {
        case PLANS.STANDARD:
          return value;
        case PLANS.SAFE:
          return value + value * 0.5;
        case PLANS.SUPER_SAFE:
          return value + value * 0.75;
        default:
          return value;
      }
    },
  },
});
</script>
