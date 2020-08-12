<template>
  <BaseField
    @input="validateInput"
    :base-text="TextText"
    :base-id="TextId"
    :base-name="getDefaultName"
    :base-type="`text`"
    :not-required="NotRequired"
  ></BaseField>
</template>

<script lang="ts">
import Vue from "vue";
import BaseField from "../base_field/BaseField.vue";
export default Vue.extend({
  components: { BaseField },
  name: "TextField",
  props: {
    TextText: {
      type: String
    },
    TextId: {
      type: String
    },
    TextName: {
      type: String
    },
    NotRequired: {
      type: Boolean
    }
  },
  computed: {
    getDefaultName: function() {
      if (this.TextName === undefined) {
        return this.TextText;
      } else {
        return this.TextName;
      }
    }
  },
  data() {
    return {
      pls: ``
    };
  },
  methods: {
    validateInput: function(input) {
      if (this.NotRequired) {
        let n = input;
        if (input === undefined) {
          //looking for if its required or not
          n = `n/a`;
        } else {
          n = input;
        }
        this.$emit("validating", this.getDefaultName, this.TextId, n, true);
      } else {
        if (input !== undefined && input !== "") {
          // making sure its not undefined or an empty string
          this.$emit(
            "validating",
            this.getDefaultName,
            this.TextId,
            input,
            true
          );
        } else {
          this.$emit(
            "validating",
            this.getDefaultName,
            this.TextId,
            input,
            false
          );
        }
      }
    }
  },
  mounted() {
    this.validateInput();
  }
});
</script>

<style scoped></style>
