<template>
    <form @submit.prevent="onSubmit">
        <button type="submit">
                Compare two movies.
        </button>
        <div>
            <label for="title-one">Pick first title</label>
            <input type="text" name="title-one" v-model="titleOne">
        </div>
        <div>
            <label for="title-two">Pick second title</label>
            <input type="text" name="title-two" v-model="titleTwo">
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
                titleOne: '',
                titleTwo: '',
                correctedTitleOne: '',
                correctedTitleTwo: '',
                sameActors: [],
                movieOneActors: [],
                movieTwoActors: [],
                noSameActors: false,
                apiPath: 'http://www.omdbapi.com/?t='
            }
        },
        methods: {
            onSubmit() {
                this.getAllMoviesData()
            },
            updateMovieOneTitle() {
                this.correctedTitleOne = this.titleOne.toLowerCase().split(" ").join("+");
            },
            updateMovieTwoTitle() {
                this.correctedTitleTwo = this.titleTwo.toLowerCase().split(" ").join("+");
            },
            checkForSameActors() {
                if (this.sameActors.length !== 0) {
                    this.noSameActors = false;
                } else if (this.sameActors.length === 0) {
                    this.noSameActors = true;
                }
            },
            getSameActors(actorsFromFirstMovie, actorsFromSecondMovie) {
                if (!actorsFromFirstMovie && !actorsFromSecondMovie) {
                    return;
                }

                delete this.movieOneActors;
                delete this.movieTwoActors;
                delete this.sameActors;


                this.movieOneActors = actorsFromFirstMovie.split(', ')
                this.movieTwoActors = actorsFromSecondMovie.split(', ')
                this.sameActors = _.intersection(this.movieOneActors, this.movieTwoActors)
                // console.log(this.sameActors);
                this.checkForSameActors();
            },
            getMovieOneData() {
                this.updateMovieOneTitle()
                // console.log(`${this.apiPath}${this.correctedTitleOne}`);
                return axios.get(`${this.apiPath}${this.correctedTitleOne}`);
            },
            getMovieTwoData() {
                this.updateMovieTwoTitle()
                // console.log(`${this.apiPath}${this.correctedTitleTwo}`);
                return axios.get(`${this.apiPath}${this.correctedTitleTwo}`);
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