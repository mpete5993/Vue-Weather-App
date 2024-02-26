<template>
  <header class="sticky top-0 bg-weather-primary shadow-lg">
    <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3 ">
          <i class="fa-solid fa-sun text-2xl text-icon-color"></i> 
          <p class="text-2xl">The Local Weather</p>
        </div>
      </RouterLink>
      <div class="flex gap-3 flex-1 justify-end">
         <i class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 curser-pointer"
         @click="toggleModal"></i>
         <i class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 curser-pointer" @click="addCity" v-if="route.query.preview"></i>
      </div>
      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <h1 class="text-black">About</h1>
        <p class="text-black">
          <b>NOTE</b>: Svenska Spel uses 1= Team A win; x = Draw; 2 = Team B win. Soccer fans betting in South Africa, however, must use 1 = Team A win; 2 = Draw; 3 = Team B win.
        </p><br>

        <p class="text-black">
          All results based on score at end of 90 minutes. If a match kicks off before the pool closes or is not completed normally, it is NOT declared null and void. Instead the result is determined by a random computer draw from 16 possibilities – 10 soccer experts each give a result, plus two of each of the three possible outcomes go into the draw.
        </p>
      </BaseModal>
    </nav>
  </header>

</template>

<script setup>
import {ref} from 'vue'
import { uid } from 'uid'
import { RouterLink, useRoute, useRouter } from 'vue-router' 
import BaseModal from './BaseModal.vue'
// import HomeView from './views/HomeView.vue'

const modalActive = ref(null);
const route = useRoute()
const router = useRouter()

const toggleModal = () => {
  modalActive.value = !modalActive.value;
}

const savedCities  = ref([])

const addCity = () => {

  if (localStorage.getItem('savedCities')) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
  }

  const locationObject = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng
    }
  };

  savedCities.value.push(locationObject);
  localStorage.setItem('savedCities', JSON.stringify(savedCities.value));

  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObject.id;
  router.replace({query})
}

</script>
