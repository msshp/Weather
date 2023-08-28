<script>

import axios from 'axios';

export default {
    data() {
        return {
            city: '',
            error: '',
            info: null
        }
    },
    computed: {
        cityName() {
            return "«" + this.city + "»"
        },
        showTemp() {
            return "Температура: " + this.info.main.temp;
        },
        showFeelsLike() {
            return "Ощущается как: " + this.info.main.feels_like;
        },
        showMinTemp() {
            return "Минимальная температура: " + this.info.main.temp_min;
        },
        showMaxTemp() {
            return "Максимальная температура: " + this.info.main.temp_max;
        }
    },
    methods: {
        getWeather() {
            if (this.city.trim().length < 2) {
                this.error = 'Введите более одного символа';
                return false;
            }

            this.error = '';

            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=adf34f371092ad3e0c4c10b61c284791`).then(res => (this.info = res.data))
        }
    }
}

// adf34f371092ad3e0c4c10b61c284791
</script>

<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city === '' ? 'вашем городе' : cityName }}</p>
        <input type="text" v-model="city" placeholder="Введите город">
        <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
        <button disabled v-else>Введите название города</button>
        <p class="error"> {{ error }}</p>

        <div class="weather" v-if="info != null">
            <p> {{ showTemp }}</p>
            <p> {{ showFeelsLike }}</p>
            <p> {{ showMinTemp }}</p>
            <p> {{ showMaxTemp }}</p>
        </div>
    </div>
</template>

<style scoped>
.error {
    color: #FCEC52;
    font-size: 14px;
}

.weather {
    margin-top: 48px;
}

.wrapper {
    width: 776px;
    height: 418px;
    padding: 40px;
    border-radius: 40px;
    background-color: #685C79;
    color: #fbfbfb;
    text-align: center;
}

.wrapper h1 {
    margin-top: 36px;
}

.wrapper p {
    margin-top: 16px;
}

.wrapper input {
    margin-top: 36px;
    background: transparent;
    border: 0;
    border-bottom: 2px solid #fbfbfb;
    color: #fbfbfb;
    font-size: 16px;
    padding: 6px 8px;
    outline: none;
}

.wrapper button:disabled {
    background: #6e6724;
    border: 1px solid #FCEC52;
    cursor: not-allowed;
    color: #fbfbfb;
}

.wrapper button:disabled:hover {
    transform: none;
}

.wrapper button {
    margin-top: 36px;
    background: #FCEC52;
    border: 0;
    color: #685C79;
    font-size: 14px;
    padding: 8px 16px;
    outline: none;
    border-radius: 8px;
    margin-left: 24px;
    cursor: pointer;
    transition: transform 500ms ease;
}

.wrapper button:hover {
    transform: scale(1.1) translateY(-4px);
}
</style>
