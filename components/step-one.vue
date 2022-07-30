<template>
  <div class="text-center max-w-xl">
    <h2 class="mb-2 font-bold text-4xl">{{ text.hello_there }}</h2>
    <p class="mb-2 mt-4">
      {{ text.lets_buy_insurance }}
    </p>
    <button
      class="bg-gray-800 w-44 h-12 hover:bg-gray-600 text-white font-bold py-2 px-4 rounded mt-12"
      @click="handleRedirection"
    >
      {{ text.start }}
    </button>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import { TEXT } from "~/utils/constants";

export default Vue.extend({
  name: "StepOne",
  methods: {
    handleRedirection() {
      if (this.$router) {
        this.$router.push({
          query: {
            page: "2",
          },
        });
      }
    },
  },
  setup() {
    return {
      text: TEXT,
    };
  },
});
</script>
