<template>
    <div id="app">
        <compare></compare>
    </div>
</template>

<script>
    import Compare from './components/compare/Compare.vue'

    export default {
        name: 'app',
        components: { Compare }
    }
</script>

<style lang="scss">
    @import 'styles/main';
</style>