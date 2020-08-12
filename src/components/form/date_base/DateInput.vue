<template>
  <DateBase
    @input="setRequired"
    :base-text="`Date`"
    :base-id="`date`"
    :base-name="`date`"
    :base-type="`date`"
    :not-required="NotRequired"
  ></DateBase>
</template>

<script lang="ts">
import Vue from "vue";
import DateBase from "./DateBase.vue";
export default Vue.extend({
  components: { DateBase },
  name: "DateInput",

  data() {
    return {
      Value: "",
      test: ""
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
        this.$emit("validating", "Date", n, true);
      } else {
        this.validateInput(input);
      }
    },
    validateInput: function(input: string | undefined) {
        if (input !== undefined && input !== "") {
          this.$emit("validating", "Date", "Date", input, true);
        } else {
          this.$emit("validating", "Date", "Date", input, false);
        }
      }
  },
  mounted() {
    this.setRequired(undefined);
  }
});
</script>

<style scoped></style>
