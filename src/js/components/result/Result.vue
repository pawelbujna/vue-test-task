<template>
    <div class="m-t-10 m-b-5 p-t-10 p-b-5 p-l-10 p-r-10 bg-col-primary">
        <div v-if="items.length > 0">
            <h1 class="col-w m-b-10 fs-10">Same actors in those movies: </h1>
            <p class="col-w m-b-5" v-for="item in items">
                {{item}}
            </p>
       </div>
       <p class="col-w m-b-5" v-if="!items.length">
        no results
       </p>
    </div>
</template>

<script>
    import _ from 'lodash';

    export default {
        props: [ 'items' ],
        data() {
            return {
                
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>