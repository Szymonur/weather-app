<template>
    <div class="c-weather">
        <div class="c-weather__form">
            <input type="text" v-model="city"> <button v-on:click="fetchCoordinates()">Check</button>
        </div>
        <div class="c-weather__box">
            <div class="c-weather__box__header">
                <h2>{{ coordinates.name}} <br>
                {{  currentTemperature }} &deg;C</h2>
            </div>
            <h3> Next hours  </h3>
            <div class="c-weather__box__list">
                <div v-for="e in segregatedData" v-bind:key="e.time" class="c-weather__box__list__element"> 
                     <div class="c-weather__box__list__element__time">{{ e.time }}</div>
                     <div class="c-weather__box__list__element__temperatue"> {{ e.temperature }} &deg;C</div> 
                     <div class="c-weather__box__list__element__rain"> {{ e.rain }} mm</div> 
                     <div class="c-weather__box__list__element__wind"> {{ e.windspeed }} km/h</div> 
                     <div class="c-weather__box__list__element__cloudcover"> {{ e.cloudcover }} %</div> 

                </div>
            </div>
        </div>

    </div>
</template>
<style lang="scss">
    ::-webkit-scrollbar {
        width: 1rem;
    }
    ::-webkit-scrollbar-track {
      box-shadow: inset 0 0 6px rgba(7, 52, 110, 0.3);
      border-radius: 5px;
    }
    ::-webkit-scrollbar-thumb {
      background-color: #2f34967e;
      border-radius: 20px;
    }
    .c-weather{
        width: 500px;
        min-height: 400px;
        background-color: rgba(240, 255, 255, 0.212);
        border-radius: 20px;
        padding: 20px;
        box-sizing: border-box;
        display: flex;
        flex-direction: column;
        color: #0e1138;

        &__box{
            h3{
                text-align: center;
            }
            &__header{
                text-align: center;
                padding: 20px 0 20px 0;
            }
            &__list{
                display: flex;
                overflow-x: scroll;
                justify-content: space-between;
                padding-bottom: 10px;
                &__element{
                    text-align: center;
                    min-width: 20%;
                    font-size: 1,2rem;
                    &__time{
                        font-weight: bold;
                    }
                }

            }
        }
        &__form{
            width: 100%;
            display: flex;
            justify-content: space-between;
            & input{
                background-color: #2f35965e;
                font-size: 1.5rem;
                padding: 10px;
                border-radius: 50px;
                width: 75%;
                border: none;
            }
            & button {
                background-color:  #2f3496bb;
                font-size: 1.5rem;
                padding: 10px;
                border-radius: 50px;
                width: 20%;
                border: none;
                &:hover{
                    background-color:  #2f349663;
                }
            };
        }
    }
</style>
<script>
    export default({
        data (){
            return {
                city: 'Warsaw',
                coordinates: '',
                weather: '',
                currentTemperature: '',
                segregatedData: []
            }
        },
        mounted() {
            this.fetchCoordinates();
        },
        methods:{
            CurrentTemperature(){
                const d = new Date();
                let currentHour = d.getHours();
                this.segregatedData.forEach(e => {
                    e.time == currentHour ? this.currentTemperature = e.temperature : 0;
                });
            },
            SegregateData(){ 
                this.segregatedData = []
                const d = new Date();
                let i = d.getHours();
                for (i; i < 24; i++) {
                     let element = {
                        time: '',
                        temperature: '',
                        rain: '',
                        windspeed: '',
                        cloudcover: '',

                     }
                     element.time = i
                     element.temperature = this.weather.hourly.temperature_2m[i];
                     element.rain = this.weather.hourly.rain[i];
                     element.windspeed = this.weather.hourly.windspeed_10m[i];
                     element.cloudcover = this.weather.hourly.cloudcover[i];
                     this.segregatedData.push(element);
                }

            },
            async fetchCoordinates(){
                const requestOptions = {
                    method: 'GET',
                    headers: { 'X-Api-Key': 'Y+pYikCGYE7BfbC1jT7HIg==iXvsBbGYiQ9Oopty'},
                };
                const response = await fetch('https://api.api-ninjas.com/v1/city?name=' + this.city, requestOptions);
                const jsonData = await response.json();
                this.coordinates = jsonData[0]
                if (jsonData != []){
                    this.fetchWearher();
                }
            },
            async fetchWearher(){
                const response = await fetch('https://api.open-meteo.com/v1/forecast?latitude='+ this.coordinates.latitude +'&longitude='+ this.coordinates.longitude +'&hourly=temperature_2m,rain,windspeed_10m,cloudcover&forecast_days=1&timezone=Europe%2FBerlin');
                const jsonData = await response.json();
                this.weather = jsonData;
                this.SegregateData();   
                this.CurrentTemperature();
            }  
        },

    })

</script>

