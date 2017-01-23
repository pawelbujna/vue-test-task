<template>
    <form>
        <button @click="getAllMoviesData()">
                test
        </button>
        <div>
            <label for="title-one">Pick first title</label>
            <input type="text" name="title-one">
        </div>
        <div>
            <label for="title-two">Pick second title</label>
            <input type="text" name="title-two">
        </div>
        <p v-if="noSameActors">
            There aren't same actors in these movies.
        </p>
        <p v-if="!noSameActors" v-for="actor in sameActors">
            {{actor}}
        </p>
    </form>
</template>

<script>
    import axios from 'axios';
    import _ from 'lodash';

    export default {
        data() {
            return {
                sameActors: [],
                movieOneActors: [],
                movieTwoActors: [],
                noSameActors: false
            }
        },
        methods: {
            checkForSameActors() {
                if (this.sameActors.length !== 0) {
                    this.noSameActors = false;
                } else if (this.sameActors.length === 0) {
                    this.noSameActors = true;
                }
            },
            getSameActors(movieOneActors, movieTwoActors) {
                this.sameActors = [];
                this.movieOneActors.push(movieOneActors);
                this.movieTwoActors.push(movieTwoActors);
                this.sameActors = _.intersection(this.movieOneActors, this.movieTwoActors)
                this.checkForSameActors();
            },
            getMovieOneData() {
                let movieOneTitle = [];
                return axios.get(`http://www.omdbapi.com/?t=gladiator`);
            },
            getMovieTwoData() {
                let movieTwoTitle = [];
                return axios.get(`http://www.omdbapi.com/?t=harry+potter`);
            },
            getAllMoviesData() {
                axios.all([this.getMovieOneData(), this.getMovieTwoData()])
                    .then(axios.spread((movieOne, movieTwo) => {
                        this.getSameActors(movieOne.data.Actors, movieTwo.data.Actors)
                    }));
            }
        }
    }

</script>

<style lang="sass" scoped>

</style>