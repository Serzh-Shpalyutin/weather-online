<script setup>
const props = defineProps({
    weatherInfo: {
        type: [Object, null],
        required: true,
    }
});

const today = new Date().toLocaleString('en-En', {weekday: 'short', year: 'numeric', month: 'long', day: 'numeric'});
</script>

<template>
    <div class="summary">
        <div :style="`background-image: url('/weather-main/${weatherInfo?.weather[0].description}.png');`"
            class="pic-main"></div>
        <div class="weather">
            <div class="temp">
                {{ Math.round(weatherInfo?.main.temp) }} °C
            </div>
            <div class="weather-desc text-block">
                {{ weatherInfo?.weather[0].description }}
            </div>
        </div>
        <div class="city text-block">
            {{ weatherInfo?.name }},
            {{ weatherInfo?.sys?.country }}
        </div>
        <div class="date text-block">
            {{ today }}
        </div>
    </div>
</template>

<style scoped>
.weather-desc::first-letter {
    text-transform: uppercase;
}
</style>