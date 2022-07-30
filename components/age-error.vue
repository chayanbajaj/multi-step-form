<template>
  <div class="text-center max-w-xl">
    <h2 class="text-3xl font-bold">{{ text.ooops }}</h2>
    <div class="my-4">
      <p>{{ text.age_over_limit }}.</p>
      <p>{{ text.apology }}</p>
    </div>
    <Button text="Ok :(" @click="handleRedirection" class="w-64 h-12"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Button from "~/ui-core/Button.vue";
import { TEXT } from "~/utils/constants";

export default defineComponent({
  name: "AgeError",
  components: {
    Button,
  },
  methods: {
    handleRedirection() {
      if (this.$router) {
        this.$router.push({
          query: {
            page: "1",
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
