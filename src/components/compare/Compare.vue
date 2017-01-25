<template>
    <div>
        <div class="p-a-10 m-l-12 m-r-12 bg-col-primary">
            <form @submit.prevent="onSubmit">
                <div class="m-b-5">
                    <label class="label col-w" for="title-one">Pick first title</label>
                    <input class="input w-12" type="text" name="title-one" v-model="titleOne">
                </div>
                <p class="m-b-5 col-alert fs-2" v-show="wrongFirstTitle">
                    Title is wrong.
                </p>
                <div class="m-b-5">
                    <label class="label col-w" for="title-two">Pick second title</label>
                    <input class="input" type="text" name="title-two" v-model="titleTwo">
                </div>
                <p class="m-b-5 col-alert fs-2" v-show="wrongSecondTitle">
                    Title is wrong.
                </p>
                <button class="button m-t-10" type="submit">
                        Compare two movies
                </button>
            </form>
        </div>
        <div class="p-a-10 m-l-12 m-r-12 m-t-12 bg-col-primary">
            <p class="col-w" v-show="isSameValue" v-for="actor in sameActors">
                    {{actor}}
            </p>
            <p class="col-w" v-show="!noSameValue">
                There aren't same actors in these movies.
            </p>
        </div>
    </div>
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
                isSameValue: false,
                noSameValue: true,
                wrongValue: false,
                wrongFirstTitle: false,
                wrongSecondTitle: false,
                apiPath: 'http://www.omdbapi.com/?t='
            }
        },
        methods: {
            resetData() {
                this.movieOneActors = [];
                this.movieTwoActors = [];
                this.sameActors = [];
                this.isSameValue = false;
                this.noSameValue = true;
                this.wrongFirstTitle = false;
                this.wrongSecondTitle = false;
            },
            checkForSameActors() {
                if (this.sameActors.length > 0) {
                    this.isSameValue = true;
                    this.noSameValue = true;
                } else if (this.sameActors.length === 0) {
                    this.isSameValue = false;
                    this.noSameValue = false;
                }
            },
            getSameActors(actorsFromFirstMovie, actorsFromSecondMovie) {
                if (!actorsFromFirstMovie) {
                    this.wrongFirstTitle = true;
                    return;
                }
                
                if (!actorsFromSecondMovie) {
                    this.wrongSecondTitle = true;
                    return;
                }

                this.resetData();

                this.movieOneActors = actorsFromFirstMovie.split(', ')
                this.movieTwoActors = actorsFromSecondMovie.split(', ')
                this.sameActors =  _.intersection(this.movieOneActors, this.movieTwoActors);

                this.checkForSameActors();
            },
            updateMovieOneTitle() {
                this.correctedTitleOne = this.titleOne.toLowerCase().split(" ").join("+");
            },
            updateMovieTwoTitle() {
                this.correctedTitleTwo = this.titleTwo.toLowerCase().split(" ").join("+");
            },
            onSubmit() {
                this.getAllMoviesData()
            },
            getMovieOneData() {
                this.updateMovieOneTitle()
                return axios.get(`${this.apiPath}${this.correctedTitleOne}`);
            },
            getMovieTwoData() {
                this.updateMovieTwoTitle()
                return axios.get(`${this.apiPath}${this.correctedTitleTwo}`);
            },
            getAllMoviesData() {
                axios.all([this.getMovieOneData(), this.getMovieTwoData()])
                    .then(axios.spread((movieOne, movieTwo) => {
                        console.log('request is done');
                        this.getSameActors(movieOne.data.Actors, movieTwo.data.Actors)
                    }));
            }
        }
    }

</script>

<style lang="sass" scoped>

</style>