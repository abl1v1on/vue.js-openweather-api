<script>
    import axios from 'axios'

    export default {
        data() {
            return {
                city: '',
                weather_info: '',
                weather_icon: '',
                isLoading: false
            }
        },

        methods: {
            async sendRequest() {
                try {    
                    this.isLoading = true

                    const {data} = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=85c9a66fb2a3bfaf771efe7e71769311&lang=ru&units=metric`)
                    console.log(data)

                    this.weather_info = data
                    this.weather_icon = `http://openweathermap.org/img/w/${data.weather[0].icon}.png`
                } catch (ex) {
                    this.weather_info = 'Город не найден'
                } finally {
                    this.isLoading = false
                }
            }
        }
    }
</script>   

<template>
<div style="max-width: 1000px; margin: 0 auto; margin-top: 170px;" class="container">
    <h1 style="color: white;" class="text-center mb-4">Weather online</h1>
    <div class="text-center mb-4">
            <div class="text-center mb-4">
                <input v-model="city" name="city" style="max-width: 300px; margin: 0 auto;" type="text" class="form-control" placeholder="Введите город" required>
            </div>
            <button @click="sendRequest" style="max-width: 300px; margin: 0 auto;" class="btn btn-outline-info w-100" type="button">Узнать погоду</button>
    </div>
        <!-- loader -->
        <div v-if="isLoading" style="text-align: center ;">
            <div class="loader">
                <div class="loader_inner"></div>
            </div>
        </div>

        <div v-show="!isLoading" v-if="weather_info !== '' && weather_info !== 'Город не найден'" class="card shadow-0 border" style="max-width: 300px; margin: 0 auto;">
            <div class="card-body p-4">
                <h4 class="mb-1 sfw-normal"></h4>
                <p class="mb-2">Текущая температура: <strong>{{ weather_info.main.temp }}°C</strong></p>
                <p>Чувствуется как: <strong>{{ weather_info.main.feels_like }}°C</strong></p>
                <p>Макс: <strong>{{ weather_info.main.temp_max }}°C</strong>, Мин: <strong> C</strong></p>
                
                <div class="d-flex flex-row align-items-center">
                    <p class="mb-0 me-4"></p>
                    <img :src="weather_icon" alt="img">
                    <i class="fas fa-cloud fa-3x" style="color: #eee;"></i>
                </div>
            </div>
        </div>
        <div v-else-if="weather_info === 'Город не найден'">
            <h1 style="color: white; text-align: center;">Город не найден</h1>
        </div>

</div>
</template>

<style scoped>
.loader {
  bottom: 0;
  left: 0;
  right: 0;
  top: 0;
  height: 100%;
  width: 100%;
  z-index: 9999;
}
 
.loader .loader_inner {
  background-image: url("../public/loader.gif");
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  height: 60px;
  width: 60px;
  margin-top: -30px;
  margin-left: -30px;
  left: 50%;
  top: 50%;
  position: absolute;
}
</style>
