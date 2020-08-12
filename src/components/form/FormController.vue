<template>
  <form-base
    :cta-text="CtaText"
    :hide-form="HideForm"
    :is-modal="IsModal"
    :form-reactor="FormReactor"
    :Bjn="Bjn"
    :form-type="FormType"
    :added-fields="AddFields"
    :button-text="ButtonText"
    :client-link="ClientLink"
    v-on:modal-control="ModalSwitch()"
  ></form-base>
</template>

<script lang="ts">
import Vue from "vue";
import FormBase from "./form_base/FormBase.vue";
export default Vue.extend({
  name: "FormController",
  components: { FormBase },
  data() {
    return {
      FormType: 2,
      FormReactor: `FRT472ABB2C5B9B141A4A3680F53877F30CAFBD9CCB917DAD1D088CEB24F2A3B2F0`,
      Bjn: `6126885173`,
      AddFields: undefined, //EXAMPLE [{placeholder: `address`, id: `address`, NotRequired: true,}]
      ButtonText: undefined,
      CtaText: undefined,
      ClientLink: "",
      IsModal: false,
      HideForm: false
    };
  },
  methods: {
    ModalSwitch: function() {
      this.$store.commit(
        "form_modal/setStatus",
        (this.HideForm = !this.HideForm)
      );
      this.HideForm = this.$store.state.form_modal.formHidden;
    }
  }
});
</script>

<style scoped lang="scss"></style>
