<template>
    <div id="app">
        <div class="row" :key="i" v-for="(row, i) in row_photos" :style="rowStyle">
            <div class="col" v-for="(photo,j) in row" :key="j" :style="colStyle">
                <div :style="[{backgroundImage: 'url(' + photo.post_url + ')'}]" ></div>
            </div>
        </div>
    </div>
</template>

<script>
    //TODO - Add on resize : change nbCol (+ recalc ?)
    //TODO FIND A WAY TO CHANGE PORTRAIT STYLE
    /*eslint no-console: 0*/
    import Photos from './photos.json';

    export default {
        name: 'app',
        props: {
            nbCol: {
                default: 4,
                type: Number
            },
            breakpoints: {
                type: Array,
                default: () => [{
                        width:'576',
                        nbCol: 2
                    },
                    {
                        width:'768',
                        nbCol: 3
                    },
                    {
                        width:'992',
                        nbCol: 4
                    }]
            },
            rowHeight: {
                default: 250,
                type: Number
            },
            colPadding: {
                default: 5,
                type: Number
            }
        },
        computed: {
            colStyle () {
                return 'padding: ' + this.colPadding + 'px ' + this.colPadding/2 + 'px' ;
            },
            rowStyle () {
                return 'height: ' + (this.rowHeight+this.colPadding) + 'px' ;
            },
            portraitStyle () {
                return this.portrait ? 'height: ' + (this.rowHeight*2+this.colPadding)+ 'px !important' : '';
            },
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
            addPicture(){ //TODO Optimize this part (it looks ugly) + workout first photo as portrait bug
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
            moveEmpty(){
                console.log('Remove Empty');
                this.photos = this.photos.filter((photo) => {
                   return photo.post_url !== ''
                });
            },
            addEmpty(){ //TODO See if this could be rethinked by using reduce
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
            },
            resized(width){ //TODO Add throttle
                this.breakpoints.forEach( (breakpoint) => {
                    if(width >= breakpoint.width){
                        this.nbCol = breakpoint.nbCol;
                    }
                })
            }
        },
        mounted() {
            this.getPictures();
            this.getPictures();
            this.getPictures();
            this.getPictures();
            window.addEventListener('resize', () => {
                this.resized(window.innerWidth)
            });
            window.addEventListener('scroll', () => {
                if ((window.innerHeight + window.pageYOffset) >= document.body.offsetHeight - 250) {
                    this.getPictures();
                }
            });
        }
    }
</script>

<style>
    img {
        max-width: 100%;
    }

    .row {
        display: flex;
    }

    .row .col {
        max-width: 100%;
        flex-grow: 1;
    }

    .col > div {
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
        height: 250px;
    }
</style>