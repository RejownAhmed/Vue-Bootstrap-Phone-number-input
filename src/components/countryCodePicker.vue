<script setup>
import { computed, ref, watch } from 'vue';
import countries from "../utils/country";

const props = defineProps({
  defaultCountry: String
});

const emit = defineEmits(['update']);

const selectedCountry = ref({});

const searchQuery = ref("");

const filteredcountries = computed(() => {
    if (searchQuery.value) {
      return countries.filter((code) => {
        const searchLower = searchQuery.value.toLowerCase();
        let foundMatch = false;
        const checkObject = (obj) => {
          Object.keys(obj).forEach((key) => {
            const value = obj[key];
            if (typeof value === 'object') {
              checkObject(value);
            } else if (typeof value === 'string' || typeof value === 'number') {
              if (value.toString().toLowerCase().includes(searchLower)) {
                foundMatch = true;
              }
            }
          });
        };
        checkObject(code);
        return foundMatch;
      });
    }
    return countries;
});

const selectCountry = (country)=> {
  selectedCountry.value = country;
  emit("update", country);
}

watch(()=> props.defaultCountry, ()=>{
    let country = countries[0];
    if (props.defaultCountry) {
      country = countries.filter(o => o.code.toString().toLowerCase() === props.defaultCountry.toString().toLowerCase())[0]
    }
    
    selectCountry(country);

}, {immediate: true})

</script>

<template>
<div class="dropdown">
  <button class="btn btn-transparent dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
    {{ `${selectedCountry.flag} ${selectedCountry.dial_code}` }}
  </button>
  <ul class="dropdown-menu">
    <li class="px-2">
      <input v-model="searchQuery" type="text" class="form-control form-control-md" placeholder="Search here">
    </li>
    <div class="menu-container overflow-scroll border-top mt-3">
      <li
        v-for="(country, index) in filteredcountries"
        :key="index"
      >
      <button class="btn btn-transparent px-2 py-2 w-100 text-start" @click="selectCountry(country)">
        {{ country.flag+' '+country.dial_code+' ('+country.code+' - '+country.name+')' }}
      </button>
      </li>

    </div>
  </ul>
</div>
</template>

<style>
.menu-container{
    min-height: 150px;
    max-height: 300px;
    min-width: 280px;
    max-width: 280px;
    overflow: auto;
}
</style>