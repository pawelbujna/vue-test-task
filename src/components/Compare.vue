<template>
    <!--<form>
        <div>
            <label for="title-one">Pick first title</label>
            <input type="text" name="title-one">
        </div>
        <div>
            <label for="title-two">Pick second title</label>
            <input type="text" name="title-two">
        </div>
        <div>
            <p v-for="movie in movieOneData">
                {movie}
            </p>
        </div>
        <p>
        </p>
    </form>-->
    <div>
        <p v-for="movie in movieOneData">
            {{movie}}
        </p>
        <button @click="submit()">
                test
        </button>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        data() {
            return {
                movieOneData: [],
                movieTwoData: []
            }
        },
        methods: {
            submit() {
                axios.get(`http://www.omdbapi.com/?t=gladiator`)
                    .then((response) => {
                        this.movieOneData = response.data.Actors.split(',')
                    })
            }
        }

    }

</script>

<style lang="sass" scoped>

</style>