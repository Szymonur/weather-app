<template>
    <div>
        <input type="text" v-model="city"> <button v-on:click="fetchCoordinates()">Check</button>
        <h2>{{ city }}</h2>
        <pre>
            {{ coordinates }}
            {{ weather }}
        </pre>
    </div>
</template>

<script>
    export default({
        data (){
            return {
                city: '',
                coordinates: '',
                weather: ''
            }
        },
        methods:{
            async fetchCoordinates(){
                const requestOptions = {
                    method: 'GET',
                    headers: { 'X-Api-Key': 'Y+pYikCGYE7BfbC1jT7HIg==iXvsBbGYiQ9Oopty'},
                };
                const response = await fetch('https://api.api-ninjas.com/v1/city?name=' + this.city, requestOptions);
                const jsonData = await response.json();
                this.coordinates = jsonData[0]
                console.log(jsonData );
                if (jsonData != []){
                    this.fetchWearher();
                }
            },
            async fetchWearher(){
                const response = await fetch('https://api.open-meteo.com/v1/forecast?latitude='+ this.coordinates.latitude +'&longitude='+ this.coordinates.longitude +'&hourly=temperature_2m,rain,windspeed_10m&forecast_days=1&timezone=Europe%2FBerlin');
                const jsonData = await response.json();
                this.weather = jsonData
            }  
        },

    })

</script>

<style lang="scss">
</style>