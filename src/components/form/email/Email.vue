<template>
  <BaseField
    @input="setRequired"
    :base-text="`Email`"
    :base-id="`Email`"
    :base-name="`email`"
    :base-type="`email`"
    :not-required="NotRequired"
  ></BaseField>
</template>

<script lang="ts">
import Vue from "vue";
import BaseField from "../base_field/BaseField.vue";
export default Vue.extend({
  components: { BaseField },
  name: "Email",

  data() {
    return {
      Value: ""
    };
  },
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
        this.$emit("validating", "Email", n, true);
      } else {

        this.validateInput(input);
      }
    },
    validateInput: function(input: string | undefined) {
      const emailPattern = /^[\w.-]+@([\w-]+\.)+[a-zA-Z]+$/;
      if (emailPattern.test(input as string)) {
        console.log("validEmail " + input);
        this.$emit("validating", "Email", "Email", input, true);
      } else {
        this.$emit("validating", "Email", "Email", input, false);
      }
    }
  },
  mounted() {
    this.setRequired(undefined);
  }
});
</script>

<style scoped></style>
