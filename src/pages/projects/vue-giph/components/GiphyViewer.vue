<template>
    <div>
        <div class="search-box">
            <input
                type="text"
                v-model="term"
                v-on:keyup.enter="searchGiphy()"
            />
            <b-button
                class="float-right"
                variant="primary"
                @click="searchGiphy()"
                >Search
                </b-button> 
        </div>
         <b-button
                size="sm"
                class="float-centre"
                @click="trendGiphy()"
            >Trending gifs
            </b-button>
            
              <b-button

                size="sm"
                class="float-centre"
                @click="randomGiphy()"
            >Random
            </b-button>
        <div>
            
        </div>
        <b-card-group columns>
            <b-card
                v-for="gif in gifs"
                :key="gif"
                :img-src="gif.images.fixed_width.url"
                :img-alt="gif.title"
            >
                <b-card-text>
                    <a :href="gif.url" target="_blank"> {{ gif.title }} </a>
                </b-card-text>
            </b-card>
        </b-card-group>
    </div>
</template>

<script>
import axios from 'axios';

const GIPHY_URL = "https://api.giphy.com/v1/gifs";
const API_KEY ="rtUEqKHrdAVyjVKpLLfET40SgfH3XTW6";

export default {
    name: 'GiphyViewer',
    data() {
        return {
            gifs: [],
            term: ""
        };
    },
    mounted() {
        this.trendGiphy();  
    },
    methods: {

        trendGiphy() {
            axios.get(`${GIPHY_URL}/trending?api_key=${API_KEY}`)
            .then((response) => {
                console.log(response.data.data)
                this.gifs = response.data.data
            })
            .catch(error => console.log(error))
        },

        randomGiphy() {
            axios.get(`${GIPHY_URL}/random?api_key=${API_KEY}`)
            .then((response) => {
                console.log(response.data.data)
                this.gifs = [response.data.data]
                //this.gifs = []
                //this.gifs.push(response.data.data)
            })
            .catch(error => console.log(error))
        },

        searchGiphy() {
            if(!this.term){
                //this.$bvToast.toast('Please enter a search term!', {
                //title: 'Warning',
                //variant: 'danger',
                //toaster: 'b-toaster-top-center',
                //autoHideDelay: 5000,
                //solid: true
                //})
                alert("Please enter a search term!")
                return
            }

            
            axios.get(`${GIPHY_URL}/search?api_key=${API_KEY}&q=${this.term}&limit=20`)
            .then((response) => {
                console.log(response.data.data)
                this.gifs = response.data.data
            })
            .catch(error => console.log(error))

            this.term = ""
        }
    }
}
</script>

<style >

</style>        