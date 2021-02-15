<template>
  <div class="widget">
    <div class="setings">
      <div class="setting__inner">
        <div class="setting__btn" @click="seting = !seting">⚙</div>
        <div class="setting__menu" v-if="seting">
          <div class="setting__menu_inner">
            <label class="city__label" for="">изменить город</label>
            <input class="menu__input" v-model="city" type="text" />
            <button class="menu__btn" @click="getWeather">
              изменить
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="main" v-if="data">
      <div class="current">
        <div class="sity">{{ cityName }}</div>
        <div class="temp">{{ temper | floor }}˚</div>
        <div>{{ weather }}</div>
      </div>
      <div class="details">
        <div class="details__elem">
          <span class="details__title">По ощущениям:</span>
          <span class="details__value">{{ temper_like | floor }} ˚</span>
        </div>
        <div class="details__elem">
          <span class="details__title">давление:</span>
          <span class="details__value">{{ pressure }} мм. рт. ст.</span>
        </div>
        <div class="details__elem">
          <span class="details__title">влажность:</span>
          <span class="details__value">{{ humidity }} %</span>
        </div>
        <div class="details__elem">
          <span class="details__title">скорость ветра:</span>
          <span class="details__value">{{ wind }} м/с</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'weather',
  data() {
    return {
      data: null,
      seting: false,
      city: 'казань',
      appKey: '258b65fe9b3210759e09601797aa2a23',
    }
  },
  computed: {
    cityName() {
      return this.data ? this.data['name'] : null
    },
    temper_like() {
      return this.data ? this.data['main']['feels_like'] : null
    },
    temper() {
      return this.data ? this.data['main']['temp'] : null
    },
    pressure() {
      return this.data ? this.data['main']['pressure'] : null
    },
    weather() {
      return this.data ? this.data['weather'][0]['description'] : null
    },
    humidity() {
      return this.data ? this.data['main']['humidity'] : null
    },
    wind() {
      return this.data ? this.data['wind']['speed'] : null
    },
  },
  mounted() {
    this.getWeather()
  },
  methods: {
    getWeather() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.appKey}&units=metric&lang=ru`
        )
        .then((response) => {
          console.log(response.data)
          this.data = response.data
        })
        .catch(function(err) {
          alert('Ошибка ввода города')
        })
      this.seting = false
    },
  },
  filters: {
    floor: function(val) {
      return Math.floor(val)
    },
  },
}
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
  font-family: Roboto, sans-serif;
}

.widget {
  display: inline-block;
  position: relative;
  box-sizing: border-box;
  height: 300px;
  background: -webkit-gradient(
      linear,
      left bottom,
      left top,
      from(#1f96ce),
      to(rgba(255, 255, 255, 0))
    ),
    #53a6cb;
  background: linear-gradient(360deg, #1f96ce 0%, rgba(255, 255, 255, 0) 100%),
    #53a6cb;
}
.main {
  color: #ffffff;
  height: auto;
  padding: 10px 5px;
}
.setting__btn {
  position: absolute;
  margin: 5px 5px;
  cursor: pointer;
  z-index: 5;
  color: rgb(243, 237, 237);
}
.setting__menu {
  display: block;
  color: rgb(19, 18, 18);
  width: auto;
  height: 300px;
  background: rgb(144, 212, 221);
}
.setting__menu_inner {
  padding: 20px;
  height: 150px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}
.city__label {
  font-size: 1.5em;
}
.current {
  text-align: center;
  margin-bottom: 2em;
}
.menu__input {
  width: 80%;
  background-color: white;
  height: 30px;
  background-repeat: no-repeat;
  font-size: 1.2em;
}
.menu__btn {
  width: 50%;
}
.sity {
  font-size: 3em;
}
.temp {
  font-size: 4em;
}
.details__elem {
  display: flex;
  justify-content: space-between;
  padding: 0 10px;
}
</style>
