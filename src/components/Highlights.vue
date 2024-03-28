<script setup>
import { Icon } from '@iconify/vue'
import { ref } from 'vue'

defineProps({
  currentWeather: Object
})
</script>

<template>
  <div class="highlights">
    <h1 class="highlights__header">Today's Highlights</h1>
    <div class="highlights__main_section">
      <div class="left_side">
        <div class="left_side__air_quality">
          <div class="left_side__air_quality_header">
            <p>Air Quality Index</p>
          </div>
          <div class="left_side__air_quality_info">
            <Icon width="64" icon="mdi:weather-windy" />
            <div class="left_side__air_quality_info__item">
              <p>PM2.5</p>
              <span>{{ currentWeather.current?.air_quality?.pm2_5 }}</span>
            </div>
            <div class="left_side__air_quality_info__item">
              <p>SO2</p>
              <span>{{ currentWeather.current?.air_quality?.so2 }}</span>
            </div>
            <div class="left_side__air_quality_info__item">
              <p>NO2</p>
              <span>{{ currentWeather.current?.air_quality?.no2 }}</span>
            </div>
            <div class="left_side__air_quality_info__item">
              <p>O3</p>
              <span>{{ currentWeather.current?.air_quality?.o3 }}</span>
            </div>
          </div>
        </div>
        <div class="left_side__wind">
          <div class="left_side__wind__humidity">
            <p>Humidity</p>
            <div class="left_side__wind__humidity__info">
              <Icon width="54" icon="ri:water-percent-line" />
              <h1>{{ currentWeather.current?.humidity }}<span>%</span></h1>
            </div>
          </div>
          <div class="left_side__wind__pressure">
            <p>Pressure</p>
            <div class="left_side__wind__pressure__info">
              <Icon width="54" icon="typcn:waves" />
              <h1>{{ currentWeather.current?.pressure_mb }}<span>hPa</span></h1>
            </div>
          </div>
        </div>
      </div>
      <div class="right_side">
        <div class="right_side__sunrise_set">
          <div class="right_side__sunrise_set__header">
            <p>Sunrise & Sunset</p>
          </div>
          <div class="right_side__sunrise_set__info">
            <div class="sunrise">
              <Icon width="54" icon="ph:sun-bold" />
              <div class="sunrise__info">
                <p>Sunrise</p>
                <span>{{ currentWeather.forecast?.forecastday[0].astro?.sunrise }}</span>
              </div>
            </div>
            <div class="sunset">
              <Icon width="54" icon="material-symbols:dark-mode-outline-rounded" />
              <div class="sunset__info">
                <p>Sunset</p>
                <span>{{ currentWeather.forecast?.forecastday[0].astro?.sunset }}</span>
              </div>
            </div>
          </div>
        </div>
        <div class="right_side__vis_feel">
          <div class="right_side__vis_feel__item">
            <p>Visibility</p>
            <div class="right_side__vis_feel__item__info">
              <Icon width="54" icon="mdi:eye-outline" />
              <h1>{{ currentWeather.current?.vis_km }}<span>km</span></h1>
            </div>
          </div>
          <div class="right_side__vis_feel__item">
            <p>Feels Like</p>
            <div class="right_side__vis_feel__item__info">
              <Icon width="54" icon="solar:temperature-bold" />
              <h1>{{ currentWeather.current?.feelslike_c }}<span>°C</span></h1>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <h1 class="today_at">Today at</h1>
  <div class="hourly">
    <div class="hour" v-for="hour in currentWeather.forecast?.forecastday[0].hour" :key="hour">
      <p>{{ new Date(hour.time).getHours() }}:00</p>
      <img :src="hour.condition.icon" alt="" />
      <p>{{ Math.round(hour.temp_c) }}°C</p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.today_at {
  margin: 25px 0;
}

.highlights {
  position: relative;
  height: 70%;
  padding: 100px 25px 25px 25px;
  border-radius: 25px;
  background-color: #181818;
  box-shadow:
    0 10px 15px -3px rgb(0 0 0 / 0.1),
    0 4px 6px -4px rgb(0 0 0 / 0.1);

  .highlights__header {
    position: absolute;
    top: 25px;
    left: 25px;
  }

  h1 {
    font-size: 30px;
    color: rgb(230, 230, 230);
  }

  .highlights__main_section {
    height: 100%;
    width: 100%;
    display: flex;
    gap: 25px;

    .left_side {
      height: 100%;
      width: 50%;
      display: flex;
      flex-direction: column;
      gap: 25px;

      .left_side__air_quality {
        height: 50%;
        padding: 25px;
        display: flex;
        flex-direction: column;
        gap: 10px;
        border-radius: 15px;
        background-color: rgb(17, 17, 17);

        .left_side__air_quality_header {
          width: 100%;
          display: flex;
          justify-content: space-between;
          margin-bottom: 25px;

          p {
            color: rgb(80, 80, 80);
            font-size: 20px;
          }

          span {
            padding: 4px 10px;
            font-size: 15px;
            color: rgb(56, 86, 57);
            background-color: rgb(131, 185, 132);
            border-radius: 25px;
          }
        }

        .left_side__air_quality_info {
          display: flex;
          align-items: center;
          justify-content: space-between;

          .left_side__air_quality_info__item {
            display: flex;
            align-items: center;
            flex-direction: column;

            p {
              color: rgb(80, 80, 80);
              margin-bottom: 10px;
            }

            span {
              font-size: 50px;
            }
          }
        }
      }

      .left_side__wind {
        height: 50%;
        display: flex;
        gap: 25px;

        p {
          color: rgb(80, 80, 80);
          font-size: 20px;
        }

        .left_side__wind__humidity {
          width: 50%;
          padding: 25px;
          display: flex;
          flex-direction: column;
          border-radius: 15px;
          background-color: rgb(17, 17, 17);

          justify-content: space-between;

          .left_side__wind__humidity__info {
            display: flex;
            align-items: center;
            justify-content: space-between;

            h1 {
              font-size: 45px;
              span {
                font-size: 30px;
              }
            }
          }
        }

        .left_side__wind__pressure {
          width: 50%;
          padding: 25px;
          display: flex;
          flex-direction: column;
          gap: 10px;
          border-radius: 15px;
          background-color: rgb(17, 17, 17);
          align-content: center;

          justify-content: space-between;

          .left_side__wind__pressure__info {
            display: flex;
            align-items: center;
            justify-content: space-between;

            h1 {
              font-size: 45px;
              span {
                font-size: 30px;
              }
            }
          }
        }
      }
    }

    .right_side {
      height: 100%;
      width: 50%;
      display: flex;
      flex-direction: column;
      gap: 25px;

      .right_side__sunrise_set {
        height: 50%;
        padding: 25px;
        display: flex;
        flex-direction: column;
        border-radius: 15px;
        background-color: rgb(17, 17, 17);

        .right_side__sunrise_set__header {
          width: 100%;
          display: flex;
          justify-content: space-between;
          margin-bottom: 40px;
        }

        p {
          color: rgb(80, 80, 80);
          font-size: 20px;
        }

        .right_side__sunrise_set__info {
          display: flex;
          justify-content: space-between;

          .sunrise {
            display: flex;
            gap: 25px;
            align-items: center;

            .sunrise__info {
              display: flex;
              flex-direction: column;

              p {
                font-size: 15px;
              }

              span {
                font-size: 50px;
              }
            }
          }

          .sunset {
            display: flex;
            gap: 25px;
            align-items: center;

            .sunset__info {
              display: flex;
              flex-direction: column;

              p {
                font-size: 15px;
              }

              span {
                font-size: 50px;
              }
            }
          }
        }
      }

      .right_side__vis_feel {
        height: 50%;
        width: 100%;
        display: flex;
        gap: 25px;
        justify-content: space-between;

        .right_side__vis_feel__item {
          width: 50%;
          display: flex;
          flex-direction: column;
          padding: 25px;
          border-radius: 15px;
          background-color: rgb(17, 17, 17);
          justify-content: space-between;

          p {
            color: rgb(80, 80, 80);
            font-size: 20px;
          }

          .right_side__vis_feel__item__info {
            display: flex;
            justify-content: space-between;
            align-items: center;

            h1 {
              font-size: 45px;
              span {
                font-size: 30px;
              }
            }
          }
        }
      }
    }
  }
}

.hourly {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 25px;

  .hour {
    width: 100px;
    display: flex;
    flex-direction: column;
    gap: 10px;
    align-items: center;
    padding: 25px;
    border-radius: 15px;
    background-color: #181818;
    box-shadow:
      0 10px 15px -3px rgb(0 0 0 / 0.1),
      0 4px 6px -4px rgb(0 0 0 / 0.1);
  }
}

@media (min-width: 0px) and (max-width: 768px) {
  .highlights {
    .highlights__main_section {
      flex-direction: column;
      .left_side {
        width: 100%;
        .left_side__air_quality {
          width: 100%;
          .left_side__air_quality_header {
            p {
              margin: 0 auto;
              font-size: 15px;
            }
          }

          .left_side__air_quality_info {
            svg {
              width: 32px;
              height: 32px;
            }

            .left_side__air_quality_info__item {
              p {
                font-size: 15px;
              }
              span {
                font-size: 15px;
              }
            }
          }
        }

        .left_side__wind {
          width: 100%;
          display: flex;
          flex-direction: column;

          .left_side__wind__humidity {
            svg {
              width: 32px;
              height: 32px;
            }

            width: 100%;
            p {
              font-size: 15px;
              margin: 0 auto;
              margin-bottom: 25px;
            }

            .left_side__wind__humidity__info {
              h1 {
                font-size: 30px;
                span {
                  font-size: 15px;
                }
              }
            }
          }

          .left_side__wind__pressure {
            svg {
              width: 32px;
              height: 32px;
            }

            width: 100%;
            p {
              font-size: 15px;
              margin: 0 auto;
              margin-bottom: 25px;
            }

            .left_side__wind__pressure__info {
              h1 {
                font-size: 30px;
                span {
                  font-size: 15px;
                }
              }
            }
          }
        }
      }

      .right_side {
        width: 100%;
        .right_side__sunrise_set {
          width: 100%;
          .right_side__sunrise_set__header {
            p {
              font-size: 15px;
              margin: 0 auto;
            }
          }
          .right_side__sunrise_set__info {
            gap: 25px;
            flex-direction: column;

            svg {
              width: 32px;
              height: 32px;
            }

            .sunrise {
              .sunrise__info {
                gap: 10px;
                p {
                  font-size: 15px;
                }
                span {
                  font-size: 20px;
                }
              }
            }

            .sunset {
              .sunset__info {
                gap: 10px;
                p {
                  font-size: 15px;
                }
                span {
                  font-size: 20px;
                }
              }
            }
          }
        }

        .right_side__vis_feel {
          flex-direction: column;
          width: 100%;
          .right_side__vis_feel__item {
            width: 100%;
            gap: 25px;
            p {
              font-size: 15px;
            }

            .right_side__vis_feel__item__info {
              svg {
                width: 32px;
                height: 32px;
              }
              h1 {
                font-size: 30px;
                span {
                  font-size: 15px;
                }
              }
            }
          }
        }
      }
    }
  }

  .hourly {
    width: 100%;
    flex-wrap: nowrap;
    overflow-x: scroll;
    padding: 25px;
    .hour {
      p {
        font-size: 15px;
      }
      img {
        width: 32px;
        height: 32px;
      }
    }
  }
}

@media (min-width: 769px) and (max-width: 1200px) {
  .highlights {
    .highlights__main_section {
      flex-direction: column;
      .left_side {
        width: 100%;
        .left_side__air_quality {
          width: 100%;
          .left_side__air_quality_header {
            p {
              margin: 0 auto;
              font-size: 20px;
            }
          }

          .left_side__air_quality_info {
            svg {
              width: 48px;
              height: 48px;
            }

            .left_side__air_quality_info__item {
              p {
                font-size: 20px;
              }
              span {
                font-size: 20px;
              }
            }
          }
        }

        .left_side__wind {
          width: 100%;
          display: flex;
          flex-direction: row;

          .left_side__wind__humidity {
            svg {
              width: 48px;
              height: 48px;
            }

            width: 100%;
            p {
              font-size: 20px;
              margin: 0 auto;
              margin-bottom: 25px;
            }

            .left_side__wind__humidity__info {
              h1 {
                font-size: 30px;
                span {
                  font-size: 15px;
                }
              }
            }
          }

          .left_side__wind__pressure {
            svg {
              width: 48px;
              height: 48px;
            }

            width: 100%;
            p {
              font-size: 20px;
              margin: 0 auto;
              margin-bottom: 25px;
            }

            .left_side__wind__pressure__info {
              h1 {
                font-size: 30px;
                span {
                  font-size: 15px;
                }
              }
            }
          }
        }
      }

      .right_side {
        width: 100%;
        .right_side__sunrise_set {
          width: 100%;
          .right_side__sunrise_set__header {
            p {
              font-size: 20px;
              margin: 0 auto;
            }
          }
          .right_side__sunrise_set__info {
            gap: 25px;
            flex-direction: row;

            svg {
              width: 48px;
              height: 48px;
            }

            .sunrise {
              .sunrise__info {
                gap: 10px;
                p {
                  font-size: 20px;
                }
                span {
                  font-size: 20px;
                }
              }
            }

            .sunset {
              .sunset__info {
                gap: 10px;
                p {
                  font-size: 20px;
                }
                span {
                  font-size: 20px;
                }
              }
            }
          }
        }

        .right_side__vis_feel {
          flex-direction: row;
          width: 100%;
          .right_side__vis_feel__item {
            width: 100%;
            gap: 25px;
            p {
              font-size: 20px;
            }

            .right_side__vis_feel__item__info {
              svg {
                width: 48px;
                height: 48px;
              }
              h1 {
                font-size: 30px;
                span {
                  font-size: 20px;
                }
              }
            }
          }
        }
      }
    }
  }

  .hourly {
    width: 100%;
    flex-wrap: nowrap;
    overflow-x: scroll;
    padding: 25px;
    .hour {
      p {
        font-size: 20px;
      }
      img {
        width: 32px;
        height: 32px;
      }
    }
  }
}

@media (min-width: 1201px) and (max-width: 1760px) {
  .hourly {
    flex-wrap: nowrap;
    overflow-x: scroll;
    padding: 25px;
  }

  .highlights {
    width: 99%;

    .highlights__main_section {
      gap: 10px;

      .left_side {
        gap: 10px;

        .left_side__air_quality {
          justify-content: space-between;

          .left_side__air_quality_info {
            svg {
              width: 48px;
              height: 48px;
            }

            .left_side__air_quality_info__item {
              p {
                font-size: 15px;
              }
              span {
                font-size: 25px;
              }
            }
          }
        }

        .left_side__wind {
          gap: 10px;
          .left_side__wind__humidity {
            .left_side__wind__humidity__info {
              justify-content: start;
              margin: 0 auto;
              svg {
                height: 48px;
                width: 48px;
              }

              h1 {
                font-size: 40px;
                span {
                  font-size: 25px;
                }
              }
            }
          }

          .left_side__wind__pressure {
            .left_side__wind__pressure__info {
              svg {
                height: 48px;
                width: 48px;
              }

              h1 {
                font-size: 40px;
                span {
                  font-size: 25px;
                }
              }
            }
          }
        }
      }

      .right_side {
        gap: 10px;

        .right_side__sunrise_set {
          gap: 20px;
          .right_side__sunrise_set__header {
            margin-bottom: 0px;
          }

          .right_side__sunrise_set__info {
            .sunrise {
              svg {
                height: 48px;
                width: 48px;
              }

              .sunrise__info {
                span {
                  font-size: 30px;
                }
              }
            }

            .sunset {
              svg {
                height: 48px;
                width: 48px;
              }

              .sunset__info {
                span {
                  font-size: 30px;
                }
              }
            }
          }
        }

        .right_side__vis_feel {
          .right_side__vis_feel__item {
            .right_side__vis_feel__item__info {
              svg {
                width: 48px;
                height: 48px;
              }

              h1 {
                font-size: 35px;

                span {
                  font-size: 20px;
                }
              }
            }
          }
        }
      }
    }
  }
}
</style>
