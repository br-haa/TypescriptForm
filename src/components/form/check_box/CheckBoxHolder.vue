<template>
  <div class="check-box-holder">
    <check-box @input="validateInput"></check-box>
    <div>
      <p class="consent-text">
        {{ ConsentText }}
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import CheckBox from "./CheckBox.vue";
export default Vue.extend({
  components: { CheckBox },
  name: "CheckBoxHolder",
  props: {
    ConsentText: {
      type: String,
      default:
        "I understand and agree that submitting this form does not create an attorney client relationship and the information I submit is not confidential or privileged"
    }
  },

  methods: {
    validateInput(CheckStatus?: boolean) {
      if (CheckStatus) {
        this.$emit("validating", "Consent", "Consent", "true", true);
      } else {
        this.$emit("validating", "Consent", "Consent", "false", false);
      }
    }
  },
  mounted() {
    this.validateInput();
  }
});
</script>

<style scoped lang="scss">
.check-box-holder {
  grid-column: span 2;
  display: flex;
  align-items: center;
  p {
    margin-left: 1rem;
    font-size: 1rem;
  }
}
</style>
