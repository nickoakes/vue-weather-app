<template>
    <div>
        <div v-show="showResults">
            <div class="container text-center">
                <div class="row">
                    <div class="col-3">
                        <img :src="flagURL">
                        <h4>
                            {{searchLocation}}
                        </h4>
                        <p>
                            <b>Current temperature: </b>{{currentTempCelsius}}
                        </p>
                        <p>
                            <b>Daily maximum: </b> {{maxTempCelsius}}
                        </p>
                        <p>
                            <b>Daily minimum: </b> {{minTempCelsius}}
                        </p>
                        <p>
                            <b>Sunrise: </b> {{sunriseTime}}
                        </p>
                        <p>
                            <b>Sunset: </b> {{sunsetTime}}
                        </p>
                    </div>
                    <div class="col-9">
                        <iframe 
                        :src="mapURL"
                        width="100%" 
                        height="350" 
                        frameborder="0" 
                        style="border:0"></iframe>
                    </div>
                </div>
            </div>
        </div>
        <div v-show="showError">
            <div class="row">
                <div class="col-12 text-center">
                    <p class="text-danger">
                        {{this.error}}
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import EventBus from '../../event-bus.js';
import moment from 'moment';

export default {
    name: 'SearchResults',
    data(){
        return{
            results: "",
            error: "",
            showResults: false,
            showError: false
        }
    },
    methods:{
        kelvinToCelsius(temp){
            return `${Math.floor(temp - 273)}°C`;
        }
    },
    computed:{
        searchLocation(){
            if(this.results){
                return `${this.results.name}, ${this.results.sys.country}`;
            } else{
                return "";
            }
        },
        flagURL(){
            if(this.results){
                return `https://www.countryflags.io/${this.results.sys.country}/shiny/64.png`;
            } else{
                return "";
            }
        },
        currentTempCelsius(){
            if(this.results){
                return `${this.kelvinToCelsius(this.results.main.temp)}`;
            } else{
                return "";
            }
        },
        maxTempCelsius(){
            if(this.results){
                return `${this.kelvinToCelsius(this.results.main.temp_max)}`;
            } else{
                return "";
            }
        },
        minTempCelsius(){
            if(this.results){
                return `${this.kelvinToCelsius(this.results.main.temp_min)}`;
            } else{
                return "";
            }
        },
        sunriseTime(){
            if(this.results){
                return moment(this.results.sys.sunrise).format("H:mm a");
            } else{
                return "";
            }
        },
        sunsetTime(){
            if(this.results){
                return moment(this.results.sys.sunset).format("H:mm a");
            } else{
                return "";
            }
        },
        mapURL(){
            if(this.results){
                return `https://maps.google.com/maps?q=${this.results.coord.lat},${this.results.coord.lon}&z=11&output=embed`;
            } else{
                return "";
            }
        }
    },
    mounted(){
        EventBus.$on('searchPerformed', (results) => {
            this.results = results.data;
            this.showError = false;
            this.error = "";
            this.showResults = true;
        });

        EventBus.$on('searchError', (error) => {
            this.results = "";
            this.showResults = false;
            this.error = error;
            this.showError = true;
        })
    }
}
</script>

<style scoped>

</style>