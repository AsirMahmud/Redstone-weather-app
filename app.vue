<script setup lg="ts">
const search = ref("Dhaka");
const input = ref("");
const searchEngine = () => {
  const formatedSearch = input.value.trim().split(" ").join("+");
  search.value = formatedSearch;
  input.value = "";
  refresh();
};
const imgUrl = ref("");
// const { data: city, error } = useFetch(
//   () =>
//     `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&APPID=f3d371a5b94174f9d7e04cad33cb91b5`
// );

const {
  data: city,
  error,
  refresh,
} = useAsyncData("city", async () => {
  const response = await $fetch(
    `https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&APPID=f3d371a5b94174f9d7e04cad33cb91b5`
  );

  const temp = response.main.temp;
  if (temp < 1) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2017/01/06/19/15/soap-bubble-1958650_640.jpg";
  } else if (1 <= temp && temp < 10) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2023/09/10/22/42/antarctica-8245765_1280.jpg";
  } else if (1 <= temp && temp < 10) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2023/09/10/15/33/maple-8245249_1280.jpg";
  } else if (11 <= temp && temp < 15) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2023/09/09/09/03/cheetah-8242729_640.png";
  } else if (16 <= temp && temp < 20) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2016/01/08/11/57/butterflies-1127666_1280.jpg";
  } else if (21 <= temp && temp < 22) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2015/04/23/22/00/tree-736885_1280.jpg";
  } else if (23 <= temp && temp < 25) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2018/08/23/07/35/thunderstorm-3625405_1280.jpg";
  } else if (26 <= temp && temp < 30) {
    imgUrl.value =
      "https://cdn.pixabay.com/photo/2016/10/25/14/03/clouds-1768967_1280.jpg";
  } else if (temp < 30) {
    imgUrl.value =
      "https://images.pexels.com/photos/296234/pexels-photo-296234.jpeg?auto=compress&cs=tinysrgb&w=300";
  }
  return response;
});
</script>

<template>
  <div class="h-screen relative">
    <!-- Background Image -->
    <div class="h-full w-full">
      <img :src="imgUrl" class="h-full w-full object-cover" />
    </div>

    <!-- Overlay -->
    <div
      class="absolute w-full h-full top-0 bg-gray-900/50 flex flex-col justify-between items-center"
    >
      <!-- Content Container -->
      <div class="w-full max-w-4xl p-4 mx-auto">
        <!-- Header Section -->
        <div class="text-center">
          <h1 class="text-white text-5xl lg:text-8xl">{{ city.name }}</h1>
          <p class="font-extralight text-2xl lg:text-4xl mt-2 text-white">
            Sunday Dec 9th
          </p>
          <img
            :src="`https://openweathermap.org/img/wn/${city.weather[0].icon}@4x.png`"
            class="icon w-20 lg:w-40 mx-auto"
          />
        </div>

        <!-- Temperature Section -->
        <div class="flex justify-center items-center mt-6">
          <p class="text-6xl lg:text-9xl font-extralight text-white">
            {{ city.main.temp }}°
          </p>
        </div>

        <!-- Search Section -->
        <div class="w-full flex flex-col items-center mt-6">
          <input
            class="input w-full lg:w-1/2 rounded-xl p-4 text-center"
            placeholder="Search for a city"
            v-model="input"
          />
          <div v-if="error">Enter Valid City</div>
          <button
            class="w-full lg:w-20 mt-2 p-2 lg:p-1 rounded-xl text-white bg-blue-500"
            @click="searchEngine"
          >
            Search
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
