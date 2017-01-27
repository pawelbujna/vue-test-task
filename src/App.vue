<template>
    <div id="app" class="height-100-vh bg-col-secondary p-t-12">
        <compare></compare>
    </div>
</template>

<script>
    import Compare from './js/components/compare/Compare.vue'

    export default {
        name: 'app',
        components: { Compare }
    }
</script>

<style lang="scss">
    @import 'styles/main';
</style>