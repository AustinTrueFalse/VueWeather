<script>
import axios from 'axios'


  export default {
    data () {
      return {
        selected: "",
        error: "",
        info: null
      }
    },
    computed: {
      cityName() {
        return this.selected
      },
      showTemp() {
        return "Температура:" + this.info.data.main.temp
      },
      showFeelsTemp() {
        return "Ощущается как:" + this.info.data.main.feels_like
      },
      showMinTemp() {
        return "Минимальная темература:" + this.info.data.main.temp_min
      },
      showMaxTemp() {
        return "Максимальная темература:" + this.info.data.main.temp_min
      }
    },
    methods: {
      getWeather() {
        if(this.selected.trim().length < 2) {
          this.error = 'Нужно название более 1 символа'
          return false
        } else {
          this.error = ""

          axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + this.selected + '&units=metric&appid=ca351ddc26fc0fb09989a95f12595004')
          .then(res => (this.info = res))
        }
        

        
      }
    },
  }

</script>

<template>
  <div class="wrapper">
    <h1>Погодное приложение</h1>
    <p>Узнать погоду в {{ selected == "" ? "вашем городе" : cityName }}</p>
    <div id="test">
      <select 
      v-model="selected"
      class="form-select">
      <option disabled value="">Выберете город</option>
      <option>Volgograd</option>
      <option>Bikin</option>
      <option>Khabarovsk</option>
    </select>
    <button 
      v-if="selected != ''"
      @click="getWeather()"
    >Узнать погоду
    </button>
    <button disabled v-else>Введите название города</button>
    </div>
    
    <p class="error"> {{ error }}</p>
    <div 
      v-if="info !=null"
      class="weather"
    >
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsTemp }}</p>
      <p>{{ showMaxTemp }}</p>
      <p>{{ showMinTemp }}</p>
    </div>
   

  </div>
</template>

<style scoped>
  .error {
    color: rgb(117, 0, 0);
  }
  .wrapper {
    width: 500px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background-color: #222222;
    text-align: center;
    color: #fff;
  }

  .wrapper h1 {
    margin-top: 50px;
  }

  .wrapper p {
    margin-top: 10px;
    font-size: 20px;
  }

  .wrapper button:disabled {
    background: #c4c4c4;
    border: none
  }
  .wrapper button {
    margin-top: 10px;
    background: #fd7ce1;
    color: #fff;
    border-radius: 10px;
    border: 2px solid #fd7ce1;
    padding: 10px;
    cursor: pointer;
    transition: transform 500ms ease;
  }

    .wrapper button:hover {
      transform: scale(1.1) translateY(-5px);
  }


</style>
