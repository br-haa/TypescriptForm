<template>
  <DateBase
    @input="validateInput"
    :base-text="`Date`"
    :base-id="`date`"
    :base-name="`date`"
    :base-type="`date`"
    :not-required="NotRequired"
  ></DateBase>
</template>

<script>
import DateBase from "./DateBase";
export default {
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
    validateInput: function(input) {
      if (this.NotRequired === true) {
        let n = input;
        if (input === undefined) {
          n = `n/a`;
        } else {
          n = input;
        }
        this.$emit("validating", "Date", n, true);
      } else {
        if (input !== undefined && input !== "") {
          this.$emit("validating", "Date", "Date", input, true);
        } else {
          this.$emit("validating", "Date", "Date", input, false);
        }
      }
    }
  },
  mounted() {
    this.validateInput();
  }
};
</script>

<style scoped></style>
