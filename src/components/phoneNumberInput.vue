<script setup>
import countryCodePicker from "./countryCodePicker.vue";
import { ref, watch } from "vue";

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

watch([phoneNumber, selectedCountry], ()=> {
  if (
    phoneNumber.value &&
    phoneNumber.value.length > selectedCountry.value?.dial_code?.length
  ) {
    originalNumber.value = phoneNumber.value?.substring(
      String(selectedCountry.value.dial_code).length,
      phoneNumber.value.length
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
