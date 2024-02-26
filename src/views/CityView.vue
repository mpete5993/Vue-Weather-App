<template>
  <div class="">
    <Suspense>
      <AsyncCityView />
      <template #fallback>
        <CityViewSkelton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
  import AsyncCityView from '../components/AsyncCityView.vue'
  import CityViewSkelton from '../components/CityViewSkelton.vue'

</script>
