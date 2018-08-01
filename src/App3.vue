<template>
    <div id="app">
        <img src="./assets/logo.png"  @click=getPictures>
        <template v-for="index in nbPhotos" >
            <div class="row" v-if="index%4==0" :key="index">
                <img :class=photos[index-1].display :src=photos[index-1].img alt="" class="col">
            </div>
            <div v-else :key="index">
                <img :class=photos[index-1].display :src=photos[index-1].img alt="" class="col">
            </div>
        </template>
    </div>
</template>

<script>
    import axios from 'axios'
    axios.defaults.baseURL = 'https://picsum.photos';

    export default {
        name: 'app',
        computed: {
            nbPhotos() {
                return this.photos.length;
            }
        },
        data () { return {
            posts: [],
            nbCol: 4,
            photos: [
                {
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },
                {
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },
                {
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                },{
                    "img": "http://www.placehold.it/500x800",
                    "display": "portrait",
                    "empty": true //or false
                }
            ]
        }
        },
        methods: {
            randomNumber: () => {
                return Math.floor(Math.random() * 3000) + 400;
            },
            getPictures () {
                let height = this.randomNumber();
                let width = this.randomNumber();
                axios.get('https://picsum.photos/' + height + '/' + width)
                    .then(response => {
                        console.log(response.data);
                    })
            }
        },
        created() {
            this.nbPhotos = this.photos.length;
            console.log(this.photos[0].img)
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

    [src="./assets/logo.png"]{
        cursor: pointer;
    }
</style>