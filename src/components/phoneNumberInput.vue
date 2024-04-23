<script setup>
import countryCodePicker from "./countryCodePicker.vue";
import { ref, watch } from "vue";

const props = defineProps({
    placeholder: String
});

const phoneCountry = defineModel("phoneCountry", {
  type: String,
});

const phoneNumber = defineModel("phoneNumber", {
  type: [String, Number],
});

const originalNumber = ref("");
const selectedCountry = ref("");

const updateValues = () => {
  phoneCountry.value = selectedCountry.value.code;
  phoneNumber.value = selectedCountry.value.dial_code + originalNumber.value;

};

const selectCountry = (country) => {
  selectedCountry.value = country;
};

watch([phoneNumber, selectedCountry], ([newPhone, newCountry], [prevPhone, prevCountry])=> {
  if (newPhone) {
    originalNumber.value = newPhone.substring(
      // To prevent the country code selector cutting extra or less part of the whole number
      // first time the phone number needs to be cut by the newCountry
      // and from the second time if we remove the previous country dial code this
      // will be the new original number
      // To understand this, think practically
      String(prevCountry ? prevCountry.dial_code : newCountry.dial_code).length,
      newPhone.length
    );
    updateValues();
  }
})
</script>

<template>
  <div>
    <div class="input-group">
      <div class="input-group-text p-0">
        <country-code-picker
          :countries="countries"
          :default-country="phoneCountry"
          @update="selectCountry"
        />
      </div>
      <input
        type="number"
        v-model="originalNumber"
        @input="updateValues()"
        class="form-control"
        placeholder="Phone number"
      />
    </div>
    <!-- {{ phoneCountry }}
    {{ phoneNumber }} -->
  </div>
</template>
