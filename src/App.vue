<script setup>
import Navbar from "./components/Navbar.vue";
import {ref,computed} from "vue";
import dayjs from "dayjs";
  const loading = ref(true);
    const dataDate = ref(null);
    const stats = ref([]);
    const countries = ref([]);
const fetchCovidData = async () => {
    loading.value = true;
      const res = await fetch('https://api.covid19api.com/summary');
      const result = await res.json();
      loading.value = false;
      dataDate.value = result.Date;
      countries.value = result.Countries;
      stats.value = result.Global;
};
fetchCovidData();

function formate(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
};
</script>
<template>
<div>
    <Navbar/>
    <div>
    <div v-if="loading" class="grid place-items-center mt-48">
    <h1 class="font-bold text-4xl ">Loading....</h1>
    </div>
<div v-else>
        <div class="grid place-items-center py-5">
        <h2 class="font-bold text-3xl">Date: {{  computed(() => dayjs(dataDate)
        .format('MMMM D YYYY, h:mm:ss a'))}}</h2>
    </div>
    <div class="grid grid-cols-1 md:grid md:grid-cols-2 w-full gap-2 pt-3 px-2">
    <div class="bg-blue-100 w-full h-96 rounded-md">
        <div class="grid items-center justify-center pt-36">
            <h2 class="font-bold text-3xl">New Confirmed:     {{ formate(stats.NewConfirmed) }}</h2>
            <h2 class="font-bold text-3xl mt-4">Total Confirmed:   {{ formate(stats.TotalConfirmed )}}</h2>
        </div>
    </div>
    <div class="bg-blue-100 w-full h-96 rounded-md">
        <div class="grid items-center justify-center pt-36">
        <h2 class="font-bold text-3xl">New Deaths:    {{ formate(stats.NewDeaths) }}</h2>
        <h2 class="font-bold text-3xl mt-4">Total Deaths:     {{ formate(stats.TotalDeaths )}}</h2>
        </div>
    </div>
</div>
</div>
</div>
</div>
</template>
<style>

</style>
