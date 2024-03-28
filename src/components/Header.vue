<script setup>
import { Icon } from '@iconify/vue'
import { inject, watch, ref } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'
import { onClickOutside } from '@vueuse/core'

const { currentWeather } = inject('searchInput')

const cityList = ref([])
const searchedCity = ref('')

const input = ref()
const popup = ref()
const search = ref()

const showPopup = () => {
  if (searchedCity.value) {
    popup.value.classList.add('active')
    input.value.classList.add('active')
  } else {
    popup.value.classList.remove('active')
    input.value.classList.remove('active')
    input.value.value = ''
    searchedCity.value = ''
  }
}

const onChangeSearchInput = debounce((event) => {
  searchedCity.value = event.target.value
}, 300)

const fetchTodayWeather = async (city) => {
  try {
    await axios
      .get(
        `http://api.weatherapi.com/v1/forecast.json?key=${import.meta.env.VITE_API_KEY}&q=${city}&days=7&aqi=yes&alerts=yes`
      )
      .then((res) => {
        currentWeather.value = res.data
      })
  } catch (err) {
    console.log(err)
  } finally {
    input.value.value = ''
    cityList.value = ''
    searchedCity.value = ''
    popup.value.classList.remove('active')
    input.value.classList.remove('active')
  }
}

const fetchSearchList = async () => {
  if (searchedCity.value == 0) {
    showPopup()
  } else {
    try {
      await axios
        .get(
          `http://api.weatherapi.com/v1/search.json?key=9e18123f3f9248479f6131609242003&q=${searchedCity.value}`
        )
        .then((res) => {
          cityList.value = res.data
          console.log(cityList.value)
        })
    } catch (err) {
      console.log(err)
    } finally {
      showPopup()
    }
  }
}

watch(searchedCity, () => {
  fetchSearchList()
})

onClickOutside(popup, () => {
  popup.value.classList.remove('active')
  input.value.classList.remove('active')
})
</script>

<template>
  <header class="header">
    <div ref="search" class="search">
      <input
        @click="showPopup()"
        ref="input"
        @input="onChangeSearchInput($event)"
        class="search__input"
        type="text"
        placeholder="search for..."
      />
      <div ref="popup" class="popup">
        <div
          v-if="cityList.length > 0"
          @click="fetchTodayWeather(city.url)"
          class="popup__city"
          v-for="city in cityList"
          :key="city"
        >
          <Icon width="16" icon="material-symbols:location-on-outline-rounded" />
          <div class="popup__city__info">
            <p>{{ city.name }}</p>
            <span>{{ city.country }}</span>
          </div>
        </div>
        <div v-else class="popup__no_city">
          <p>No city found</p>
        </div>
      </div>
    </div>
  </header>
</template>

<style lang="scss" scoped>
.header {
  margin-bottom: 25px;
  display: flex;
  width: 100%;
  justify-content: center;

  .search {
    width: 25%;
    position: relative;
    .search__input {
      width: 100%;
      background-color: #181818;
      padding: 25px;
      border: none;
      border-top-left-radius: 15px;
      border-top-right-radius: 15px;
      &.active {
        border-bottom-left-radius: 0px !important;
        border-bottom-right-radius: 0px !important;
      }
      border-bottom-left-radius: 15px;
      border-bottom-right-radius: 15px;
      border-bottom: 1px solid transparent;
      color: white;
      outline: none;
      &::placeholder {
        color: rgb(77, 77, 77);
      }
    }
  }

  .search__icon {
    right: 5px;
    top: 20px;
    position: absolute;
    cursor: pointer;
    margin-right: 4px;
    width: 25px;
    height: 25px;
  }
}
.popup {
  width: 100%;
  visibility: hidden;
  z-index: 10;
  position: absolute;
  display: flex;
  flex-direction: column;
  left: 0;
  top: 65px;
  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
  background-color: #181818;
  box-shadow:
    0 10px 15px -3px rgb(0 0 0 / 0.4),
    0 4px 6px -4px rgb(0 0 0 / 0.4);

  .popup__no_city {
    padding: 15px;
    display: flex;
    gap: 10px;
    align-items: center;
  }

  .popup__city {
    padding: 15px;
    display: flex;
    gap: 10px;
    align-items: center;
    cursor: pointer;
    &:hover {
      background-color: #252525;
      transition: 0.3s;
    }

    .popup__city__info {
      display: flex;
      flex-direction: column;

      span {
        color: rgb(88, 88, 88);
      }
    }

    &:last-child {
      border-bottom-left-radius: 15px;
      border-bottom-right-radius: 15px;
    }
  }

  &.active {
    visibility: visible;
  }
}

@media (max-width: 1200px) {
  .header {
    .search {
      width: 50%;
    }
  }
}
</style>
