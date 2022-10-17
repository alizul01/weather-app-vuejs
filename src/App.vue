<template>
  <div class="bg-slate-600">
    <div class="flex justify-center h-screen bg-cover">
      <img
        id="shadowIn"
        class="fixed h-screen bg-cover"
        src="./assets/coldBg.jpg"
      />
      <div class="z-10 w-[32rem] flex flex-col">
        <input
          type="text"
          class="p-4 rounded-lg mt-8 bg-white/70 w-full font-normal text-slate-700 placeholder:text-slate-700 outline-none hover:scale-105 focus:scale-105 hover:shadow-sm focus:shadow-lg duration-300 ease-in-out"
          placeholder="Search Country"
          v-model="query"
          @keypress="fetchData"
        />

        <div
          id="weatherWrapper"
          class="mt-8"
          v-if="
            weather.weather &&
            weather.weather.length > 0 &&
            weather.weather[0].main
          "
        >
          <div class="location-box">
            <div
              class="text-white text-3xl font-semibold text-center drop-shadow-md"
            >
              {{ weather.name }}, {{ weather.sys.country }}
            </div>
            <div class="text-white text-xl italic font-regular text-center">
              {{ dateBuilder() }}
            </div>
          </div>

          <div id="weather-box">
            <div
              class="inline-block py-4 px-6 text-white text-8xl bg-white/20 rounded-xl shadow-lg my-4 font-bold"
            >
              {{ Math.round(weather.main.temp) }}°C
            </div>
            <div class="text-white text-2xl font-regular">
              {{ weather.weather[0].main }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      api_key: "823f0f65ed8aaae8cfd39daacfcb4056",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },

  methods: {
    fetchData(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults)
          .catch((err) => {
            console.log(err);
          });
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = new Date();
      return `${days[day.getDay()]}, ${
        months[day.getMonth()]
      } ${day.getDate()} ${day.getFullYear()}`;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#bgImage {
  background-image: url("./assets/coldBg.jpg");
}

#shadowIn {
  filter: brightness(0.7);
}
</style>
