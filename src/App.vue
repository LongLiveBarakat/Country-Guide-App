<template>
  <div class="container">

    <div class="search-box">
      <input type="text" placeholder="Search For Country.." v-model="searchTerm">
      <button @click="searchCountry">Search</button>
    </div>

    <div class="country-flag">
      <img id="flag" :src="countryData.flag" alt="">
      <h1 id="country-name"> {{ countryData.name }} </h1>
    </div>

    <div class="info">
      <h5>Capital: {{ countryData.capital }} </h5>
      <h5>Continent: {{ countryData.region }} </h5>
      <h5>Population: {{ countryData.population }} </h5>
      <h5 v-if="countryData.currencies.length > 0">
        Currency: {{ countryData.currencies[0].name }} ({{ countryData.currencies[0].code }})
      </h5>
      <h5 v-else>
        Currency information not available.
      </h5>
      <h5>Common Languages: {{ countryData.languages.join(', ') }} </h5>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const searchTerm = ref('');

const countryData = ref({
  flag: '',
  name: '',
  capital: '',
  region: '',
  population: '',
  currencies: [],
  languages: [],
});

const searchCountry = async () => {
  try  {
    const response = await fetch(`https://restcountries.com/v3.1/name/${searchTerm.value}?fullText=true`);
    const data = await response.json();

    if (data.length > 0) {
      countryData.value = {
        flag: data[0].flags.png,
        name: data[0].name.common,
        capital: data[0].capital[0],
        region: data[0].region,
        population: data[0].population,
        currencies: Object.values(data[0].currencies),
        languages: Object.values(data[0].languages),
      };
    } else {
      console.log("country data was not found");
    }
  } catch (error) {
    console.log('Error fetching country data:', error);
  }
};

</script>

<style scoped>
* {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.container {
  width: 600px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  background: beige;
  padding: 3rem;
  border-radius: 20px;
}

.container .search-box {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.container .search-box input {
  outline: none;
  background: beige;
  background-color: beige;
  border: none;
  border-bottom: 1px solid black;
  padding: 5px;
}

.container .search-box button {
  outline: none;
  background: beige;
  border: 1px solid black;
  width: 100px;
  height: 40px;
  border-radius: 50px;
}

.container .country-flag {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 2rem;
}

.container .country-flag img {
  width: 50%;
  margin-bottom: 1rem;
}

.container .info h5 {
  font-weight: 400;
  margin-bottom: 1rem;
}

</style>
