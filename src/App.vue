<template>
  <div id="app">
    <FormController></FormController>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import FormController from "@/components/form/FormController.vue";
export default Vue.extend({
  name: "App",
  components: { FormController }
});
</script>

<style lang="scss"></style>
