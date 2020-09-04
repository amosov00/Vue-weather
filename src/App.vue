<template>
  <div id="app">
      <video-bg :sources="[bgVideo + `.mp4`]" :key="render" class="videobg">
        <app-input v-on:myinput="value = $event" v-on:cleaninput="value = ''" :newmyinput="value"></app-input>
        <div class="weather" v-if="city == ''" style="font-size: 30px">Введите любой город</div>
        <div class="weather" v-else>
          <p style="font-size: 40px; margin: 0">{{city + ', ' + country}}</p>
          <div class="weather__tempicon">
            <p style="font-size: 60px; margin: 0">{{temp + '&deg;С'}}</p>
            <img v-bind:src="'http://openweathermap.org/img/wn/' + icon + '@2x.png'" alt="icon" />
          </div>
          <p style="font-size: 25px; margin: 0">{{description}}</p>
        </div>
      </video-bg>
  </div>
</template>

<script>
import VideoBg from "vue-videobg";
import axios from "axios";
import { debounce } from "./utils/debounce";
export default {
  data() {
    const dbnc = debounce(async () => {
      await axios
        .get(
          "http://api.openweathermap.org/data/2.5/weather?q=" +
            this.value +
            "&appid=1b2d1aa4a79b62d7b3639edfb73cd869"
        )
        .then((resolve) => (this.axiosValue = resolve))
        .then(
          (resolve) => (
            (this.description = resolve.data.weather[0].description),
            (this.temp = Math.floor(resolve.data.main.temp - 273.15)),
            (this.country = resolve.data.sys.country),
            (this.city = resolve.data.name),
            (this.icon = resolve.data.weather[0].icon)
          )
        )
        .catch((reject) => (this.reject = reject));
    }, 400);

    return {
      value: "",
      axiosValue: "",
      reject: "",
      description: "",
      temp: "",
      country: "",
      city: "",
      icon: "",
      render: 1,
      dbnc,
    };
  },
  watch: {
    value() {
      this.dbnc();
    },
    city() {
      this.render++;
    },
  },
  components: { VideoBg },
  computed: {
    bgVideo() {
      return this.icon == '01d' ? 'Clear' :
            this.icon == '01n' ? 'Clear' :
            this.icon == '02d' ? 'Clouds' :
            this.icon == '02n' ? 'Clouds' :
            this.icon == '03d' ? 'pasmurn-sky' :
            this.icon == '03n' ? 'pasmurn-sky' :
            this.icon == '04d' ? 'pasmurn-sky' :
            this.icon == '04n' ? 'pasmurn-sky' :
            this.icon == '10d' ? 'Rain' :
            this.icon == '09n' ? 'Rain' :
            this.icon == '09d' ? 'Rain' :
            this.icon == '10n' ? 'Rain' :
            this.icon == '11d' ? 'litning' :
            this.icon == '13d' ? 'snow' :
            this.icon == '13n' ? 'snow' :
            this.icon == '50d' ? 'fog' :
            this.icon == '50n' ? 'fog' :
            this.icon == '11n' ? 'litning'  : 'de';
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Nunito:wght@600&display=swap");

body {
  margin: 0;
  padding: 0;
  font-family: "Nunito", sans-serif;
}

.weather {
  background-color: rgb(0%, 0%, 0%, 0.5);
  min-height: 250px;
  max-width: 400px;
  border-radius: 25px;
  margin: auto;
  margin-top: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  color: white;
}

.weather__tempicon {
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-width: 200px;
}

img {
  width: 80px;
  height: 80px;
  margin: 0 -15px;
}

#app {
  display: block
}
</style>
