<template>
  <label
    class="fields"
    :for="AreaId"
    :class="{ fieldFocus: focused || userInput !== '' }"
    :style="{ gridColumn: `span ${SpanSize}` }"
  >
    <span class="fieldText">{{ SetPlaceholder() }}</span>
    <textarea
      @input="SendUp"
      @focus="focused = true"
      @blur="focused = false"
      :id="AreaId"
      :type="AreaType"
      :name="AreaName"
      v-model="userInput"
    ></textarea>
  </label>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "TextAreaBase",
  data() {
    return {
      focused: false,
      userInput: "",
      info: {
        name: this.AreaName,
        filled: false
      }
    };
  },
  props: {
    AreaText: {
      type: String
    },
    AreaId: {
      type: String
    },
    AreaName: {
      type: String
    },
    AreaType: {
      type: String,
      default: `text`
    },
    NotRequired: {
      type: Boolean
    },
    SpanSize: {
      type: Number
    }
  },
  methods: {
    SetPlaceholder: function() {
      if (this.NotRequired) {
        return this.AreaText;
      } else {
        return `*${this.AreaText}`;
      }
    },
    checkFilled() {
      this.info.filled = this.userInput === "";
    },
    SendUp: function() {
      this.$emit("input", this.userInput);
    }
  }
});
</script>

<style scoped lang="scss">
$small-size: 640px;

.fields {
  display: grid;
  .fieldText {
    grid-area: 1 / 1 / 2 / 2;
    align-self: start;
    z-index: 11;
    margin: 5px 0 0 1rem;
    cursor: text;
    transform: translateY(0);
    transition: 0.3s;
    pointer-events: none;
    user-select: none;
  }
  textarea {
    grid-area: 1 / 1 / 2 / 2;
    resize: none;
    height: 150px;
  }
}
.fieldFocus {
  .fieldText {
    grid-area: 1 / 1 / 2 / 2;
    z-index: 11;
    margin: 0 0 0 0;
    transform: translateY(-1.2rem);
    transition: 0.3s;
    font-size: 1rem;
    @media (max-width: $small-size) {
      transform: translateY(-1rem);
      font-size: 0.8rem;
    }
  }
}
input,
textarea {
  border: none;
  box-shadow: 0 0 1px 0.5px black;
  transition: 0.5s;
  padding: 0.5rem 0.5rem 0.5rem 0.5rem;
}

textarea:focus {
  outline: none;
  box-shadow: 0 0 3px 1px black;
  transition: 0.5s ease-in-out;
}
</style>
