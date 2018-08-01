<template>
    <div id="app">
        <img src="./assets/logo.png">
        <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
        <template v-for="(photo, index) in photos">
            <!--
            <div class="row" v-if="index%nbCol==0" v-bind:key="index">
                <div :class=photo.display v-bind:key="photo" class="col">
                    <img :src=photo.img alt="">
                </div>
            </div>
            -->
            <div v-bind:key="photo" class="col-3">
                <img :class=photo.display :src=photo.img alt="">
            </div>
        </template>
        <div class="btn btn-primary" @click=randomNumber>
            Click
        </div>
    </div>
</template>

<script>
    import HelloWorld from './components/HelloWorld.vue'
    import axios from 'axios'
    axios.defaults.baseURL = ''

    export default {
        name: 'app',
        components: {
            HelloWorld
        },
        data () { return {
            "nbCol": 4,
            "nbPhoto": 4,
            "photos":
                [
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    },
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    },
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    },
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    },
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    },
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    },
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    },
                    {
                        "img": "http://www.placehold.it/500x800",
                        "display": "portrait",
                        "empty": true //or false
                    }
                ]
            }
        },
        methods: {
            randomNumber: () => {
                Math.floor(Math.random() * 3000) + 400;
            },
            getPictures: () => {
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    body {
        padding: 20px;
        font-family: Helvetica;
    }

    .portrait {
        height: 20vh;
        overflow: hidden;
        object-fit: cover;
    }

    .landscape {
        height: 10vh;
        overflow: hidden;
        object-fit: cover
    }

    img {
        max-width: 100%;
    }
</style>
