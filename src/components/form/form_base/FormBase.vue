<template>
  <div>
    <error-message :received-text="errorText"></error-message>
    <thank-you
      v-if="thankYouActive"
      :activated="thankYouActive"
      :client-link="ClientLink"
    ></thank-you>
    <transition name="fade">
      <div v-if="getHideValues" v-bind:class="{ 'form-is-modal': IsModal }">
        <div id="form-outside">
          <div class="XHolder" v-if="IsModal">
            <button class="modal-close" v-on:click="$emit(`modal-control`)">
              <span class="theXFactor"></span>
            </button>
          </div>
          <transition name="in-out" appear>
            <div id="form-inside" v-if="getHideValues">
              <div class="cta-text">
                <h2>{{ CtaText }}</h2>
              </div>
              <form id="contact-form">
                <div class="form1" v-if="FormType === 1">
                  <text-field
                    @validating="trackValidation"
                    :text-text="`First Name`"
                    :text-id="'FirstName'"
                  ></text-field>
                  <text-field
                    @validating="trackValidation"
                    :text-text="`Last Name`"
                    :text-id="'LastName'"
                  ></text-field>
                  <email @validating="trackValidation"></email>
                  <phone-number @validating="trackValidation"></phone-number>
                  <div class="addedFieldsGrid" v-if="AddedFields">
                    <text-field
                      class="BasicField"
                      @validating="trackValidation"
                      v-for="field in AddedFields"
                      :text-text="field.placeholder"
                      :key="field.id"
                      :text-id="field.id"
                      :text-name="field.name"
                      :not-required="field.NotRequired"
                    ></text-field>
                  </div>
                </div>

                <div class="form2" v-if="FormType === 2">
                  <div class="name-grid">
                    <text-field
                      @validating="trackValidation"
                      :text-text="`First Name`"
                      :text-id="'FirstName'"
                    ></text-field>
                    <text-field
                      @validating="trackValidation"
                      :text-text="`Last Name`"
                      :text-id="'LastName'"
                    ></text-field>
                  </div>
                  <email @validating="trackValidation"></email>
                  <phone-number @validating="trackValidation"></phone-number>
                  <text-field
                    @validating="trackValidation"
                    :text-text="`Address`"
                    :text-id="'Address'"
                  ></text-field>
                  <text-field
                    @validating="trackValidation"
                    :text-text="`City`"
                    :text-id="'City'"
                  ></text-field>
                  <div class="state-zip-grid">
                    <StateDropdown
                      @validating="trackValidation"
                    ></StateDropdown>
                    <Zip @validating="trackValidation"></Zip>
                  </div>
                  <div class="addedFieldsGrid" v-if="AddedFields">
                    <text-field
                      class="BasicField"
                      @validating="trackValidation"
                      v-for="field in AddedFields"
                      :text-text="field.placeholder"
                      :key="field.id"
                      :text-id="field.id"
                      :text-name="field.name"
                      :not-required="field.NotRequired"
                    ></text-field>
                  </div>
                </div>

                <div class="form3" v-if="FormType === 3">
                  <div class="name-grid">
                    <text-field
                      @validating="trackValidation"
                      :text-text="`First Name`"
                      :text-id="'FirstName'"
                    ></text-field>
                    <text-field
                      @validating="trackValidation"
                      :text-text="`Last Name`"
                      :text-id="'LastName'"
                    ></text-field>
                  </div>
                  <email @validating="trackValidation"></email>
                  <phone-number @validating="trackValidation"></phone-number>
                  <text-field
                    @validating="trackValidation"
                    :text-text="`Address`"
                    :text-id="'Address'"
                  ></text-field>
                  <text-field
                    @validating="trackValidation"
                    :text-text="`City`"
                    :text-id="'City'"
                  ></text-field>
                  <div class="state-zip-grid">
                    <StateDropdown
                      @validating="trackValidation"
                    ></StateDropdown>
                    <Zip @validating="trackValidation"></Zip>
                  </div>
                  <DateInput
                    class="long-span"
                    @validating="trackValidation"
                  ></DateInput>
                  <StateDropdown
                    class="long-span"
                    @validating="trackValidation"
                    :duplicate="2"
                    :state-text="undefined"
                  ></StateDropdown>
                  <div class="addedFieldsGrid" v-if="AddedFields">
                    <text-field
                      class="BasicField"
                      @validating="trackValidation"
                      v-for="field in AddedFields"
                      :text-text="field.placeholder"
                      :key="field.id"
                      :text-id="field.id"
                      :text-name="field.name"
                      :not-required="field.NotRequired"
                    ></text-field>
                  </div>
                </div>

                <text-area
                  @validating="trackValidation"
                  :text-text="`Message`"
                  :text-id="'Message'"
                  :not-required="true"
                  :form-type="FormType"
                ></text-area>
                <check-box-holder
                  @validating="trackValidation"
                  v-if="FormType === 1 || FormType === 3"
                ></check-box-holder>
              </form>
              <div class="buttonGrid">
                <div class="line"></div>
                <div>
                  <button id="SubmitButton" @click="FormPostStart" class="btn1">
                    {{ ButtonText }}
                  </button>
                  <!--      <button @click="formTest()">TEST</button>-->
                </div>
                <div class="line"></div>
              </div>
            </div>
          </transition>
        </div>
      </div>
    </transition>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TextField from "../text_field/TextField.vue";
import Email from "../email/Email.vue";
import PhoneNumber from "../phone_number/PhoneNumber.vue";
import ErrorMessage from "../error_message/ErrorMessage.vue";
import TextArea from "../text_area/TextArea.vue";
import CheckBoxHolder from "../check_box/CheckBoxHolder.vue";
import ThankYou from "../thank_you/ThankYou.vue";
import StateDropdown from "../state_dropdown/StateDropdown.vue";
import Zip from "../zip_field/Zip.vue";
import DateInput from "../date_base/DateInput.vue";
interface ValidationObjectInterface {
  id: string;
  name: string;
  status: boolean;
  value: string;
}
interface CtmObjectInterface {
  country_code: string;
  name: string;
  phone: string;
  email: string;
  custom: any;
}
export default Vue.extend({
  components: {
    DateInput,
    Zip,
    StateDropdown,
    ThankYou,
    CheckBoxHolder,
    TextArea,
    ErrorMessage,
    PhoneNumber,
    Email,
    TextField
  },

  name: "FormBase",
  data: () => ({
    validationObjects: [] as ValidationObjectInterface[], // the objects as they get pulled from the fields
    errorText: [] as string[], // where the error messages go
    thankYouActive: false, // triggers the thank-you page
    CtmObject: {} as CtmObjectInterface, // the final object that gets posted to CTM
    AllData: [] as ValidationObjectInterface[] // the combination of the validated objects and Post data
  }),
  props: {
    AddedFields: {
      type: Array as () => object[]
    },
    ButtonText: {
      type: String,
      default: `Get Started!`
    },
    FormType: {
      type: Number
    },
    FormReactor: {
      type: String
    },
    Bjn: {
      type: String
    },
    ClientLink: {
      type: String
    },
    IsModal: {
      type: Boolean
    },
    CtaText: {
      type: String
    },
    HideForm: {
      type: Boolean
    }
  },
  computed: {
    getHideValues: function(): boolean {
      // controls what shows up if modal is or isn't active
      if (this.IsModal) {
        return !this.HideForm;
      } else {
        return true;
      }
    }
  },
  methods: {
    trackValidation(name: string, id: string, value: string, status: boolean) {
      //pulls each of the objects from the fields or rather gets slapped with it at every button press.
      const obj = { name: name, id: id, value: value, status: status };
      let check: boolean[] = [];
      this.validationObjects.forEach((item: ValidationObjectInterface) => {
        // updates value if found
        if (item.id === id) {
          item.status = status;
          item.value = value;
          check.push(true);
        } else {
          check.push(false);
        }
      });
      if (
        check.some(x => {
          return x;
        })
      ) {
        // if not found adds to array
        check = [];
      } else {
        this.validationObjects.push(obj);
        check = [];
      }
    },

    FormPostStart() {
      this.setTimeStamp(); // updates time stamp
      if (
        this.validationObjects.some((item: { status: boolean }) => {
          return !item.status;
        })
      ) {
        // if a false is found for an object status its goes to error message
        this.errorText = [] as string[];
        this.HandleErrorMessage();
      } else {
        console.log("Posting this data"); // console logging the visible form data
        this.validationObjects.forEach((x: ValidationObjectInterface) => {
          console.log(x.name + " " + x.value);
        });
        this.CreateObject();
      }
    },

    CreateObject() {
      // creating the form post object for ctm
      this.CtmObject = {
        // eslint-disable-next-line @typescript-eslint/camelcase
        country_code: "1", // the expected country code e.g. +1, +44, +55, +61, etc... the plus is excluded
        name: this.search(`FirstName`) + " " + this.search(`LastName`),
        phone: this.search(`Phone`) as string,
        email: this.search(`Email`) as string,
        custom: {}
      };
      this.AllData = this.validationObjects.concat(
        this.$store.state.Values.PostValues
      );
      this.AllData.forEach((vo: ValidationObjectInterface) => {
        // adding all the post values and custom values to the custom object
        if (vo.id !== "Phone" && vo.id !== "Email") {
          // basically just not repeating phone and email
          this.CtmObject.custom[vo.id] = vo.value;
        }
      });
      this.PostType1();
    },

    HandleErrorMessage() {
      // error message handling. who could have guessed
      this.validationObjects.forEach((x: ValidationObjectInterface) => {
        if (!x.status) {
          this.errorText.push(`Please check ${x.name} is filled correctly`);
        }
      });
    },
    setTimeStamp() {
      this.$store.commit("Values/SortV", {
        id: `TimeStamp`,
        value: new Date().getTime(),
        destination: "post"
      });
    },

    PostType1() {
      // sending off the data
      // eslint-disable-next-line no-undef
      __ctm.form.track(
        "app.calltrackingmetrics.com",
        this.FormReactor,
        this.Bjn,
        this.CtmObject
      );
      this.ThankYouPageActivate();
    },

    search: function(id: string) {
      // grabbing object value by id name
      for (let i = 0; i < this.validationObjects.length; i++) {
        const oob: ValidationObjectInterface = this.validationObjects[i];
        if (oob.id === id) {
          return oob.value as string;
        }
      }
    },
    ThankYouPageActivate(): void {
      // activating thankyou modal
      this.thankYouActive = true;
    }
  }
});
</script>

<style scoped lang="scss">
.form1 {
  display: grid;
  grid-gap: 1.5rem;
  grid-template-columns: 1fr 1fr;
  margin-bottom: 1.5rem;
  @media (max-width: 1080px) {
    grid-template-columns: 1fr;
  }
  .addedFieldsGrid {
    grid-column: span 2;
    display: grid;
    grid-gap: 1.5rem;
    grid-template-columns: 1fr 1fr;
    @media (max-width: 1080px) {
      grid-template-columns: 1fr;
      grid-column: span 1;
    }
  }
}
.form2 {
  display: grid;
  grid-gap: 1.5rem;
  grid-template-columns: 1fr 1fr 1fr;
  margin-bottom: 1.5rem;
  @media (max-width: 1080px) {
    grid-template-columns: 1fr;
  }
  .addedFieldsGrid {
    grid-column: span 3;
    display: grid;
    grid-gap: 1.5rem;
    grid-template-columns: 1fr 1fr 1fr;
    @media (max-width: 1080px) {
      grid-template-columns: 1fr;
      grid-column: span 1;
    }
  }
}
.form3 {
  display: grid;
  grid-gap: 1.5rem;
  grid-template-columns: 1fr 1fr 1fr;
  margin-bottom: 1.5rem;
  @media (max-width: 1080px) {
    grid-template-columns: 1fr;
  }
  .long-span {
    grid-column: span 3;
    @media (max-width: 1080px) {
      grid-column: span 1;
    }
  }
  .addedFieldsGrid {
    grid-column: span 3;
    display: grid;
    grid-gap: 1.5rem;
    grid-template-columns: 1fr 1fr 1fr;
    @media (max-width: 1080px) {
      grid-template-columns: 1fr;
      grid-column: span 1;
    }
  }
}
.name-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 1.5rem;
  @media (max-width: 1080px) {
    grid-template-columns: 1fr;
  }
}

.state-zip-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 1.5rem;
  @media (max-width: 1080px) {
    grid-template-columns: 1fr;
  }
}
#contact-form {
}
.buttonGrid {
  display: grid;
  margin-top: 2rem;
  grid-template-columns: 1fr auto 1fr;
  align-items: center;
  justify-items: center;
  grid-column-gap: 1rem;
  .line {
    width: 100%;
    height: 3px;
    background: black;
  }
}
.form-is-modal {
  #form-outside {
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    background: rgba(2, 2, 19, 0.86);
    display: grid;
    padding: 1rem;
    box-sizing: border-box;
    grid-template-rows: auto 1fr;
    #form-inside {
      align-self: center;
      justify-self: center;
      box-sizing: border-box;
      background: rgba(255, 255, 255, 0.8);
      padding: 1rem 2rem;
      box-shadow: 0px 0px 7px 7px rgba(0, 0, 0, 0.66),
        0px 0px 5px 3px rgba(0, 0, 0, 0.36), 0px 0px 3px 1px rgba(0, 0, 0, 0.76);
      margin-top: -2rem;
    }
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
.in-out-enter-active {
  animation: in 0.5s;
}
.in-out-leave-active {
  animation: in 0.3s reverse;
}
@keyframes in {
  0% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}

.XHolder {
  grid-area: 1/1/2/2;
  align-self: start;
  justify-self: end;
  margin: 1.5rem 3rem;
  @media (max-width: 640px) {
    margin: 0.5rem 0.5rem;
  }
}
.modal-close {
  border: none;
  outline: none;
  background: none;
  height: 60px;
  width: 60px;
  margin: 0;
  padding: 0.5rem;
  box-sizing: border-box;
  justify-self: end;
  cursor: pointer;
  transition: 0.3s ease-out;
  filter: drop-shadow(0 1px 3px black);
  &:hover {
    transform: scale(1.2) rotate(180deg);
    transition: 0.4s ease-out;
    filter: drop-shadow(0 1px 5px black);
  }
  .theXFactor {
    display: block;
    width: 100%;
    height: 100%;
    background: yellow;
    clip-path: polygon(
      20% 0%,
      0% 20%,
      30% 50%,
      0% 80%,
      20% 100%,
      50% 70%,
      80% 100%,
      100% 80%,
      70% 50%,
      100% 20%,
      80% 0%,
      50% 30%
    );
    -webkit-clip-path: polygon(
      20% 0%,
      0% 20%,
      30% 50%,
      0% 80%,
      20% 100%,
      50% 70%,
      80% 100%,
      100% 80%,
      70% 50%,
      100% 20%,
      80% 0%,
      50% 30%
    );
  }
}
.cta-text {
  text-align: center;
}
</style>
