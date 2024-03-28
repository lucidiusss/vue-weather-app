<script setup>
import Header from './components/Header.vue'
import Forecast from './components/Forecast.vue'
import Highlights from './components/Highlights.vue'
import axios from 'axios'
import { Icon } from '@iconify/vue'

import { ref, provide, onMounted } from 'vue'

const currentWeather = ref({})
const date = ref('')

const isLoading = ref(true)

const options = {
  weekday: 'long',
  month: 'long',
  day: 'numeric'
}

provide('searchInput', {
  currentWeather
})

onMounted(async () => {
  try {
    await axios
      .get(
        `http://api.weatherapi.com/v1/forecast.json?key=${import.meta.env.VITE_API_KEY}&q=London&days=7&aqi=yes&alerts=yes`
      )
      .then((res) => {
        currentWeather.value = res.data
        date.value = new Date(res.data.forecast.forecastday[0].date).toLocaleDateString(
          undefined,
          options
        )
      })
  } catch (err) {
    console.log(err)
  } finally {
    isLoading.value = false
  }
})
</script>

<template>
  <div v-auto-animate="{ duration: 100 }">
    <div v-auto-animate v-if="!isLoading" class="app">
      <Header />
      <div class="main">
        <div class="left_side">
          <Forecast :current-weather="currentWeather" :date="date" />
        </div>
        <div class="right_side">
          <Highlights :current-weather="currentWeather" />
        </div>
      </div>
    </div>
    <div class="loading" v-else>
      <h1>loading...</h1>
      <Icon width="32" v-auto-animate icon="svg-spinners:270-ring-with-bg" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.app {
  display: flex;
  flex-direction: column;

  padding: 10px 25px;

  border-radius: 25px;
  margin: 0 auto;
  color: #fff;

  .main {
    margin-top: 25px;
    display: flex;
    gap: 25px;

    .left_side {
      display: flex;
      flex-direction: column;
      width: 20%;
    }

    .right_side {
      display: flex;
      flex-direction: column;
      width: 80%;
    }
  }

  @media (min-width: 0px) and (max-width: 768px) {
    .main {
      flex-direction: column;

      .left_side {
        width: 100%;
      }

      .right_side {
        width: 100%;
      }
    }
  }

  @media (min-width: 769px) and (max-width: 1200px) {
    .main {
      flex-direction: column;

      .left_side {
        width: 100%;
      }

      .right_side {
        width: 100%;
      }
    }
  }

  @media (min-width: 1201px) and (max-width: 1760px) {
    .main {
      gap: 10px;
    }
  }
}

.loading {
  text-align: center;
  align-items: center;
  font-size: 20px;
  color: white;
  margin: 20% auto;
  h1 {
    margin-bottom: 25px;
  }
}
</style>
