<template>
    <div class="m-t-10 m-b-5 p-t-10 p-b-5 p-l-10 p-r-10 bg-col-primary" v-if="items.length > 0">
        <h1 class="col-w m-b-10 fs-10">Movies you were looking for: </h1>
        <p class="col-w m-b-5" v-for="item in items">
            {{item}}
        </p>
    </div>
</template>

<script>
    import firebase from 'firebase';
    import _ from 'lodash';

    let firebaseRef = firebase.database().ref('/')

    export default {
        data() {
            return {
                items: []
            }
        },
        mounted() {
            firebaseRef.on('value', (response) => {
                let responseData = response.val();

                if (responseData) {
                    this.items =  Object.keys(responseData.movies).map(x => responseData.movies[x].title)
                }
            });
        }
    }
</script>

<style lang="scss" scoped>

</style>