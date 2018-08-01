<template>
    <div id="app">
        <button @click="twocol">2 col</button>
        <button @click="threecol">3 col</button>
        <button @click="fourcol">4 col</button>
        <div class="row" :key="i" v-for="(row, i) in row_photos">
            <div class="col" v-for="(photo,j) in row" :key="j">
                <div :style="{backgroundImage: 'url(' + photo.post_url + ')'}" :class="photo.portrait === true ? 'portrait' : '' " ></div>
            </div>
        </div>
    </div>
</template>

<script>
    //TODO - Add on resize : change nbCol (+ recalc ?)
    //TODO - Remove useless Code
    //TODO - Add parameters : breakpoints, hauteur etc
    /*eslint no-console: 0*/
    import Photos from './photos.json';

    export default {
        name: 'app',
        computed: {
            row_photos() {
                let rphotos = [];
                let row = [];
                this.photos.map((p,i) => {
                    if(i !== 0 && i%this.nbCol === 0){
                        rphotos.push(row);
                        row = [];
                    }
                    row.push(p);
                });
                return rphotos
            },
        },
        data () {
        return {
            nbCol: 4,
            photos: []
            }
        },
        methods: {
            randomNumber() {
                return Math.floor(Math.random() * 3000) + 400;
            },
            getPictures() {
                console.log('triggered');
                //Add row by row photos.
                for(let i=0; i < this.nbCol; i++){
                    this.addPicture();
                }
            },
            addPicture(){
                let height = this.randomNumber();
                let width = this.randomNumber();
                //Si tableau non vide
                if(this.photos.length > this.nbCol){
                    //Si nbCol photos avant est un portrait,
                    if(this.photos[this.photos.length - this.nbCol].portrait === true){
                        this.photos.push({
                            post_url: '',
                            portrait: false,
                        });
                    } else {
                        this.photos.push({
                            post_url: 'https://picsum.photos/'+height+'/'+width,
                            portrait: height >= width,
                        });
                    }
                } else {
                    this.photos.push({
                        post_url: 'https://picsum.photos/'+height+'/'+width,
                        portrait: height >= width,
                    });
                }

            },
            test(){
                fetch('http://example.com/movies.json', {
                        credentials: 'omit'
                    })
                    .then(function(response) {
                        return response.json();
                    })
                    .then(function(myJson) {
                        console.log(myJson);
                    });
            },
            fileExists(url) {
                console.log('test');
                if(url){
                    var req = new XMLHttpRequest();
                    req.open('GET', url, false);
                    req.send();
                    return req.status==200;
                } else {
                    return false;
                }
            },
            removeEmpty(){
                console.log('Remove Empty');
                this.photos = this.photos.filter((photo) => {
                   return photo.post_url !== ''
                });
                /*
                this.photos.forEach((photo, index) => {
                    console.log("before splice : " + this.photos[index].post_url);
                    if (photo.post_url === '') {
                        this.photos.splice(index, 1);
                    }
                    this.$nextTick(() => {
                        console.log("after splice " + this.photos[index].post_url);
                    })
                });
                */
            },
            addEmpty(){ //TODO See if this could be rethinked
                console.log('Add empty');
                const empty = {
                    post_url: '',
                    portrait: false,
                };
                this.photos.forEach((photo, index) => {
                    if( photo.portrait ) {
                        this.photos.splice(index + this.nbCol, 0, empty);
                        index--;
                    }
                })
                /*
                this.photos = this.photos.reduce( (photos, photo, index) => {
                    photos.push( photo );
                    if( photo.portrait ) { photos.splice(index + this.nbCol, 0, empty); }
                    return photos;
                }, []);
                */
                /*
                this.photos.reduce((accumulator, currentValue, currentIndex, array) => {
                   if(currentValue.portrait) {
                       //array.
                   }
                });
                */
                /*
                this.photos.forEach((photo, index) => {
                    if (photo.portrait == true) {
                        this.photos.splice(index + this.nbCol, 0, empty);
                    }
                })
                */
            },
            twocol(){
                this.nbCol = 2;
                this.removeEmpty();
                this.addEmpty();
            },
            threecol(){
                this.nbCol = 3;
                this.addEmpty(this.removeEmpty())
            },
            fourcol(){
                this.nbCol = 4;
                this.addEmpty(this.removeEmpty())
            }
        },
        mounted() {
            this.fileExists('photos.json');
            this.getPictures();
            this.getPictures();
            this.getPictures();
            this.getPictures();
            window.addEventListener('resize', () => {
                console.log(window.innerWidth);
                if(window.innerWidth < '500px'){
                    this.nbCol = 3;
                }
            });
            window.addEventListener('scroll', () => {
                if ((window.innerHeight + window.pageYOffset) >= document.body.offsetHeight - 250) {
                    this.getPictures();
                }
            });
        }
    }

    function throttle(callback, wait, context = this) {
        let timeout = null
        let callbackArgs = null

        const later = () => {
            callback.apply(context, callbackArgs)
            timeout = null
        }

        return function() {
            if (!timeout) {
                callbackArgs = arguments
                timeout = setTimeout(later, wait)
            }
        }
    }
</script>

<style>
    img {
        max-width: 100%;
    }

    .row {
        display: flex;
        height: 255px;
    }
    .row .col {
        max-width: 100%;
        padding: 5px 2.5px;
        flex-grow: 1;
    }

    .col > div {
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
        height: 250px;
    }

    .portrait {
        height: 505px !important;
    }
</style>