<script setup>
import { Icon } from '@iconify/vue'
import { ref } from 'vue'

defineProps({
  currentWeather: Object,
  date: String
})

const options = {
  weekday: 'short'
}

const options1 = {
  month: 'short',
  day: 'numeric'
}
</script>

<template>
  <div class="container">
    <div class="current_weather">
      <h1>Now</h1>
      <div class="current_weather__info">
        <div class="current_weather__icon">
          <div class="current_weather__temp">
            <p>{{ Math.round(currentWeather.current?.temp_c) }}</p>
            <span>°C</span>
          </div>
          <img :src="currentWeather.current?.condition?.icon" alt="weather_icon" />
        </div>
        <p>{{ currentWeather.current?.condition?.text }}</p>
      </div>
      <div class="current_weather__date">
        <div class="current_weather__day">
          <Icon width="24" icon="material-symbols:calendar-today-outline" />
          <p>{{ date.slice(0, 1).toUpperCase() + date.slice(1) }}</p>
        </div>
        <div class="current_weather__location">
          <Icon width="24" icon="material-symbols:location-on-outline-rounded" />
          <p>{{ `${currentWeather.location?.name}, ${currentWeather.location?.country}` }}</p>
        </div>
      </div>
    </div>
    <div class="forecast">
      <h1>7 Days Forecast</h1>
      <div class="forecast__weather">
        <div
          class="forecast__weather__day"
          v-for="day in currentWeather.forecast?.forecastday"
          :key="day"
        >
          <div class="forecast__weather__temp">
            <img width="48px" :src="day.day?.condition?.icon" alt="weather_icon" />
            <p>{{ Math.round(day.day?.avgtemp_c) }}°</p>
          </div>
          <div class="forecast__weather__date">
            <p>{{ new Date(day.date).toLocaleDateString(undefined, options1).slice(0, 6) }}</p>
            <p>{{ new Date(day.date).toLocaleDateString(undefined, options) }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  height: 100%;

  .current_weather {
    height: auto;
    justify-content: start;
    padding: 25px;
    border-radius: 25px;

    background-color: #181818;
    box-shadow:
      0 10px 15px -3px rgb(0 0 0 / 0.1),
      0 4px 6px -4px rgb(0 0 0 / 0.1);

    h1 {
      padding: 0;
      margin: 0;
      font-size: 30px;
      color: rgb(230, 230, 230);
    }

    .current_weather__info {
      width: 100%;
      display: flex;
      flex-direction: column;
      border-bottom: 1px solid rgb(80, 80, 80);
      align-items: start;

      padding-bottom: 15px;
      margin-bottom: 10px;

      .current_weather__icon {
        width: 100%;
        display: flex;
        justify-content: space-between;

        p {
          margin: 0;
          font-size: 80px;
        }
        span {
          margin: 0;
          font-size: 40px;
        }

        .current_weather__temp {
          display: flex;
          align-items: center;
        }
      }
    }

    .current_weather__date {
      margin-top: 20px;
      display: flex;
      flex-direction: column;
      gap: 10px;

      p {
        color: rgb(80, 80, 80);
      }

      .current_weather__day {
        display: flex;
        align-items: center;
        gap: 10px;
      }

      .current_weather__location {
        display: flex;
        align-items: center;
        gap: 10px;
      }
    }
  }

  .forecast {
    height: 60%;
    margin-top: 30px;
    h1 {
      margin-left: 25px;
      margin-bottom: 30px;
      font-size: 20px;
    }

    .forecast__weather {
      justify-content: start;
      padding: 25px;
      border-radius: 25px;
      background-color: #181818;
      box-shadow:
        0 10px 15px -3px rgb(0 0 0 / 0.1),
        0 4px 6px -4px rgb(0 0 0 / 0.1);

      .forecast__weather__day {
        display: flex;
        align-items: center;
        justify-content: space-between;

        .forecast__weather__temp {
          display: flex;
          align-items: center;
          gap: 20px;
          width: 50%;

          p {
            font-size: 35px;

            color: #fff;
          }
        }

        .forecast__weather__date {
          width: 50%;
          color: rgb(80, 80, 80);
          display: flex;
          align-items: center;
          gap: 80px;
        }

        .forecast__weather__weekday {
          color: rgb(80, 80, 80);
          display: flex;
          align-items: center;
        }
      }
    }
  }
}

@media (min-width: 0px) and (max-width: 768px) {
  .container {
    .forecast {
      .forecast__weather {
        .forecast__weather__day {
          .forecast__weather__temp {
            width: 50%;
          }

          .forecast__weather__date {
            justify-content: space-between;
            width: 50%;
          }
        }
      }
    }
  }
}

@media (min-width: 769px) and (max-width: 1200px) {
  .container {
    .forecast {
      .forecast__weather {
        .forecast__weather__day {
          .forecast__weather__temp {
            width: 50%;
          }

          .forecast__weather__date {
            justify-content: space-between;
            width: 50%;
          }
        }
      }
    }
  }
}

@media (min-width: 1201px) and (max-width: 1760px) {
  .container {
    .current_weather {
      height: auto;
    }

    .forecast {
      height: auto;

      .forecast__weather__day {
        width: 100%;
        .forecast__weather__temp {
          gap: 0px !important;
          img {
            width: 32px;
            height: 32px;
          }

          p {
            font-size: 20px !important;
          }
        }

        .forecast__weather__date {
          gap: 0px !important;
          justify-content: space-between;
          p {
            font-size: 13px !important;
          }
        }
      }
    }
  }
}
</style>
