<template>
  <div class="h-screen flex items-center justify-center">
    <StepOne v-if="page === 1" />
    <StepTwo
      v-else-if="page === 2"
      :age="age"
      :name="name"
      :pack="pack"
      :region="region"
      :resetValues="resetValues"
      @input="handleInputChange"
    />
    <Summary
      v-else-if="page === 3"
      :age="age"
      :name="name"
      :pack="pack"
      :region="region"
      :resetValues="resetValues"
    />
    <AgeError v-if="page === 'age-error'" />
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import Router from "vue-router";
import StepOne from "../components/step-one.vue";
import StepTwo from "~/components/step-two.vue";
import AgeError from "~/components/age-error.vue";
import Summary from "~/components/summary.vue";
Vue.use(Router);

export default Vue.extend({
  name: "IndexPage",
  components: { StepOne, StepTwo, AgeError, Summary },
  computed: {
    page() {
      if (!this.$route.query.page) {
        return 1;
      }
      const page = this.$route.query.page as string;
      return Number.isNaN(Number(page)) ? page : Number(page);
    },
  },
  data() {
    return {
      age: 0,
      name: "",
      pack: "Standard",
      region: "Hong Kong",
    };
  },
  methods: {
    handleInputChange(e: Event) {
      const name = (e.target as HTMLInputElement).name;
      const value = (e.target as HTMLInputElement).value;
      switch (name) {
        case "name":
          this.name = value;
          break;
        case "age":
          this.age = Number(value);
          break;
        case "pack":
          this.pack = value;
          break;
        case "region":
          this.region = value;
          break;
        default:
          break;
      }
    },
    resetValues() {
      this.age = 0;
      this.name = "";
      this.pack = "Standard";
      this.region = "Hong Kong";
    },
  },
});
</script>
