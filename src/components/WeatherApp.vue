<template>
    <div class="weather-app">
        <h1>Weather App</h1>
        <form @submit.prevent="getWeather">
            <label for="city">Enter city: </label>
            <input type="text" id="city" v-model="city" required>
            <button type="submit">Get Weather</button>
        </form>
        <div v-if="error" class="error-message">{{ error }}</div>
        <div v-if="weather" class="weather-info">
            <h2>{{ city }}</h2>
            <p>Temperature: {{ weather.main.temp }} &deg;C</p>
            <p>Weather: {{ weather.weather[0].description }}</p>
            <p>Wind Speed: {{ weather.wind.speed }} m/s</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            city: '',
            weather: null,
            error: null
        }
    },
    methods: {
        async getWeather() {
            try {
                const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=3c15a073dc05168905dbc403b2a9cbc1&units=metric`);
                const data = await response.json();
                if (data.cod === 200) {
                    this.weather = data;
                    this.error = null;
                } else {
                    this.error = data.message;
                    this.weather = null;
                }
            } catch (error) {
                console.error(error);
                this.error = 'An error occurred while fetching weather data';
                this.weather = null;
            }
        }
    }
}
</script>

<style>
.weather-app {
    margin: 20px auto;
    max-width: 600px;
    text-align: center;
}

.weather-app h1 {
    font-size: 3rem;
    margin-bottom: 20px;
}

.weather-app input[type="text"] {
    padding: 10px;
    font-size: 1.2rem;
    border-radius: 4px;
    border: none;
    border-bottom: 2px solid #ccc;
    margin-bottom: 20px;
    width: 100%;
    max-width: 400px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.weather-app input[type="text"]:focus {
    outline: none;
    border-color: #007bff;
}

.weather-app button {
    padding: 10px 20px;
    font-size: 1.2rem;
    border-radius: 4px;
    border: none;
    background-color: #007bff;
    color: #fff;
    cursor: pointer;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.weather-app button:hover {
    background-color: #0069d9;
}

.weather-app .error-message {
    color: #dc3545;
    margin-bottom: 20px;
}

.weather-app .weather-info {
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px;
    background-color: #fff;
    text-align: left;
}

.weather-app .weather-info h2 {
    font-size: 2rem;
    margin-bottom: 10px;
}

.weather-app .weather-info p
{
font-size: 1.2rem;
margin-bottom: 5px;
}
</style>