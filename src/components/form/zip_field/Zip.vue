<template>
  <BaseField
    @input="setRequired"
    :base-text="`Zip`"
    :base-id="`Zip`"
    :base-name="`zip`"
    :base-type="`zip`"
    :not-required="NotRequired"
  ></BaseField>
</template>

<script lang="ts">
import Vue from "vue";
import BaseField from "../base_field/BaseField.vue";
export default Vue.extend({
  components: { BaseField },
  name: "Zip",

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
        this.$emit("validating", "Zip", n, true);
      } else {
        this.validateInput(input);
      }
    },
    validateInput: function(input: string | undefined) {
      const emailPattern = /^[0-9]{5}(?:-[0-9]{4})?$/;
      if (emailPattern.test(input as string)) {
        console.log("validZip " + input);
        this.$emit("validating", "Zip", "Zip", input, true);
      } else {
        this.$emit("validating", "Zip", "Zip", input, false);
      }
    }
  },
  mounted() {
    this.setRequired(undefined);
  }
});
</script>

<style scoped></style>
