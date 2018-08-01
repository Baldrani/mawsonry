<template>
    <div id="app">
        <Mawsonry
            :nbCol=3
            :breakpoints="breakpoints">
        </Mawsonry>
    </div>
</template>

<script>
    /*eslint no-console: 0*/
    import Mawsonry from './components/Mawsonry'
    export default {
        name: 'app',
        data: () => {
            return {
                breakpoints: [{
                    width: 576,
                    nbCol: 2
                }]
            }
        },
        components: { Mawsonry }
    }
</script>