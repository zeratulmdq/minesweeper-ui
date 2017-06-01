<template>
    <div id="app">
        <minesweeper></minesweeper>
    </div>
</template>

<script>
import minesweeper from './minesweeper.vue'

export default {
    name: 'app',
    components: {
        minesweeper,
    },
    data () {
        return {
        }
    }
}
</script>

<style>
    #app {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100%;
    }
</style>
