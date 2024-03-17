<script setup>
import { ref } from "vue";
import phoneNumberInput from "./components/phoneNumberInput.vue";

const phone = ref({
  phone_country: "BD",
  phone_number: "+8801782599228"
});
</script>

<template>
  <div class="container-fluid py-5">
    <div class="row justify-content-center align-center">
      <div class="col-12 col-md-6 col-lg-5 col-xl-4">
        Phone number input
        <phone-number-input
          v-model:phone-country="phone.phone_country"
          v-model:phone-number="phone.phone_number"
          class="py-4"
        />
      </div>

      {{ phone }}
    </div>
  </div>
</template>
