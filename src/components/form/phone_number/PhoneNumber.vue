<template>
  <BaseField
    @input="setRequired"
    :base-text="`Phone`"
    :base-id="`Phone`"
    :base-name="`phone`"
    :base-type="`tel`"
    :not-required="NotRequired"
  ></BaseField>
</template>

<script lang="ts">
import Vue from "vue";
import BaseField from "../base_field/BaseField.vue";
export default Vue.extend({
  components: { BaseField },
  name: "PhoneNumber",
  props: {
    NotRequired: {
      type: Boolean
    }
  },
  methods: {
    setRequired(input: string | undefined) {
      if (this.NotRequired) {
        let n = input;
        if (input === undefined) {
          n = `n/a`;
        } else {
          n = input;
        }
        this.$emit("validating", "Phone", n, true);
      } else {
        this.validateInput(input);
      }
    },
    validateInput(input: string | undefined) {
      const phonePattern = /^\(?([0-9]{3})\)?[-. ]?([0-9]{3})[-. ]?([0-9]{4})$/;
      if (phonePattern.test(input as string)) {
        console.log("checking phone " + input);
        this.$emit("validating", "Phone", "Phone", input, true);
      } else {
        this.$emit("validating", "Phone", "Phone", input, false);
      }
    }
  },
  mounted() {
    this.setRequired(undefined);
  }
});
</script>

<style scoped></style>
