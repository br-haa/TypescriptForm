<template>
  <TextAreaBase
    @input="setRequired"
    :area-text="TextText"
    :area-id="TextId"
    :area-name="getDefaultName"
    :area-type="`text`"
    :not-required="NotRequired"
    :span-size="getSpanSize"
  ></TextAreaBase>
</template>

<script lang="ts">
import Vue from "vue";
import TextAreaBase from "./TextAreaBase.vue";
export default Vue.extend({
  components: { TextAreaBase },
  name: "TextArea",
  props: {
    TextText: {
      type: String,
      default: "Message"
    },
    TextId: {
      type: String,
      default: "Message"
    },
    TextName: {
      type: String,
      default: "Message"
    },
    NotRequired: {
      type: Boolean
    },
    FormType: {
      type: Number
    }
  },
  computed: {
    getDefaultName: function() {
      if (this.TextName === undefined) {
        return this.TextText;
      } else {
        return this.TextName;
      }
    },
    getSpanSize: function() {
      if (this.FormType === 1) {
        return 2;
      } else {
        return 3;
      }
    }
  },

  data() {
    return {};
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
        this.$emit("validating", this.getDefaultName, this.TextId, n, true);
      } else {
        this.validateInput(input);
      }
    },
    validateInput: function(input: string | undefined) {
      if (input !== undefined && input !== "") {
        this.$emit("validating", this.getDefaultName, this.TextId, input, true);
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
  },
  mounted() {
    this.setRequired(undefined);
  }
});
</script>

<style scoped></style>
