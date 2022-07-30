<template>
  <div class="text-center max-w-xl">
    <h2 class="mb-2 text-4xl font-bold">{{ text.about_yourself }}</h2>
    <div class="px-8 pt-6 pb-8 mb-4">
      <InputField
        label="Name"
        name="name"
        placeholder="Name"
        :value="name"
        @input="handleChange"
      />
      <InputField
        label="Age"
        min="0"
        name="age"
        placeholder="Age"
        type="number"
        :value="age"
        @input="handleChange"
      />
      <Select
        label="Where do you live"
        name="region"
        :options="regionOptions"
        @change="handleChange"
      />
      <div class="mb-4 text-left">
        <div
          v-for="(option, index) in planOptions"
          v-bind:key="index"
          class="my-2 cursor-pointer"
        >
          <label class="flex items-center">
            <input
              type="radio"
              name="pack"
              :value="option"
              :checked="pack === option"
              @change="handleChange"
            />
            <span class="ml-2 form-check-label inline-block text-gray-800">{{
              option + getExtraPremiumAmount(option)
            }}</span>
          </label>
        </div>
      </div>
      <div class="my-4 font-semibold text-3x mt-16">
        <h2>{{ text.your_premium + ": " + amount + currencyCode }}</h2>
      </div>
      <div class="mt-16">
        <Button @click="handleGoBack" text="Back" :type="'outlined'"  class="w-28 h-12"/>
        <Button @click="handleGoNext" :disabled="disabled" text="Next"  class="w-28 h-12"/>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import InputField from "~/ui-core/InputField.vue";
import Select from "~/ui-core/Select.vue";
import {
  CURRENCY_CODE,
  PLANS,
  PLAN_OPTIONS,
  REGIONS,
  REGION_OPTIONS,
} from "~/utils/constants";
import Button from "~/ui-core/Button.vue";
import { TEXT } from "~/utils/constants";

export default defineComponent({
  name: "StepTwo",
  emits: ["input"],
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
    getExtraPremiumAmount(plan: string) {
      const value: number = this.age * 10 * this.rate;
      if (!value) {
        return "";
      }
      switch (plan) {
        case PLANS.SAFE:
          return " (+" + value * 0.5 + `${this.currencyCode}, 50%)`;
        case PLANS.SUPER_SAFE:
          return " (+" + value * 0.75 + `${this.currencyCode}, 75%)`;
        default:
          return "";
      }
    },
    handleGoBack() {
      this.handleRedirection("1");
      this.resetValues();
    },
    handleGoNext() {
      if (!this.disabled) {
        if (this.age > 100) {
          this.handleRedirection("age-error");
        } else {
          this.handleRedirection("3");
        }
      }
    },
  },
  components: { InputField, Select, Button },
  computed: {
    disabled() {
      const name: string = this.name;
      const age: number = this.age;
      const isDisabled: boolean = name.trim() === "" || age === 0;
      return isDisabled;
    },
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
    extraPremium() {
      switch (this.pack) {
        case PLANS.STANDARD:
          return 0;
        case PLANS.SAFE:
          return 0.5;
        case PLANS.SUPER_SAFE:
          return 0.75;
        default:
          return 0;
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
  setup(_, { emit }) {
    const handleChange = (e: Event) => emit("input", e);
    return {
      handleChange,
      planOptions: PLAN_OPTIONS,
      regionOptions: REGION_OPTIONS,
      text: TEXT,
    };
  },
});
</script>
