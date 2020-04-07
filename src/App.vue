<template>
  <div id="app" :class="typeof weather.main != 'undefined' ? setBg(weather.weather[0].main) : ''">
    <main>
      <div class="container-fluid">
        <div class="row text-center">
          <div class="col-12 mt-5 d-flex justify-content-center flex-column align-items-center">
            <div class="search">
              <input
                type="text"
                class="search-bar"
                placeholder="Cerca una città..."
                v-model="location"
                @keypress="fetchWeather"
              />
            </div>
            <div
              class="weather-wrap"
              v-if="typeof weather.main != 'undefined' && weather.main != ''"
            >
              <div class="location-container">
                <div class="location">{{ weather.name }} ,{{ weather.sys.country }}</div>
                <div class="date">{{ new Date().toLocaleDateString("it-IT", options) }}</div>
              </div>
              <div class="weather-box">
                <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
                <div class="weather">{{ weather.weather[0].main }}</div>
              </div>
              <div class="wind">
                <img src="../src/assets/wind1.png" alt />
                {{
                weather.wind.speed
                }}
                m/s
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "16bbd468b7939364458e51eea2fccbb2",
      base_url: "https://api.openweathermap.org/data/2.5/",
      location: "",
      weather: {},
      options: {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric"
      }
    };
  },
  methods: {
    fetchWeather(e) {
      var key = e.which || e.keyCode || 0;
      if (key == 13) {
        fetch(
          `${this.base_url}weather?q=${this.location}&units=metric&APPID=${this.api_key}`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setResults)
          .then(this.reset);
      }
    },
    setBg(weather) {
      if (weather == "Clear") return "clear";
      else if (weather == "Clouds") return "clouds";
      else if (weather == "Rain") return "rain";
    },
    setResults(results) {
      this.weather = results;
      console.log(this.weather);
    },
    reset() {
      document.querySelector(".search-bar").value = "";
    }
  }
};
</script>

<style lang="scss" scoped>
$text-color-primary: white;

@mixin setBg($path) {
  background: $path;
  background-repeat: no-repeat;
  background-size: cover;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background: url("assets/121428.jpg");

  background-size: cover;
  background-attachment: fixed;
  main {
    height: 100vh;
    background-image: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.15),
      rgba(0, 0, 0, 0.55)
    );
    .search {
      width: 60%;
      .search-bar {
        color: #313131;
        font-size: 20px;
        appearance: none;
        border: none;
        outline: none;
        background: none;
        background-color: rgba(255, 255, 255, 0.5);
        border-radius: 0 16px 0 16px;
        transition: 0.4s;
        padding: 10px;
        width: 100%;
        &:focus {
          background-color: rgba(255, 255, 255, 0.75);
          box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
          border-radius: 16px 0 16px 0;
        }
      }
    }
    .location {
      color: $text-color-primary;
      font-size: 32px;
      font-weight: 600;
      text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }
    .date {
      color: $text-color-primary;
      font-size: 20px;
      font-weight: 400;
      font-style: italic;
    }
    .temp {
      padding: 10px 25px;
      color: $text-color-primary;
      font-size: 100px;
      font-weight: 900;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.25);
      border-radius: 16px;
      box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
    .weather,
    .wind {
      color: $text-color-primary;
      font-size: 50px;
      font-weight: 700;
      font-style: italic;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
  }
}
#app.rain {
  @include setBg(url("assets/storm.gif"));
}
#app.clear {
  @include setBg(
    url("https://media.giphy.com/media/mEUl5b6QiFKvHPbZm3/giphy.gif")
  );
}
#app.clouds {
  @include setBg(
    url("https://media.giphy.com/media/QU3fhTj1hc0FzofIGB/giphy.gif")
  );
}
</style>
