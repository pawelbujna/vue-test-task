<template>
        <form class="pos-relative m-b-10" @submit.prevent="getMoviesSugestions()">
                <label class="label col-w">Pick title</label>
                <input class="input w-12" type="text" v-model="title" @keyup="getMoviesSuggestions()" autocomplete="off">
                <div class="pos-absolute w-12 z-3 top-100">
                    <ul class="list">
                        <li class="list-item" v-for="sugestion in sugestions" @click="pickTitle(sugestion.imdbID, sugestion.Title)">
                            {{sugestion.Title}}
                        </li>
                    </ul>
                </div>
        </form>
</template>

<script>
    import axios from 'axios';
    import _ from 'lodash';

    export default {
        data() {
            return {
                title: '',
                sugestions: [],
                movieData: {},
                isVisible: false,
                apiSearchPath: 'http://www.omdbapi.com/?s=',
                apiIdPath: 'http://www.omdbapi.com/?i='
            }
        },
        methods: {
            pickTitle(movieId, title) {
                this.title = title;
                this.sugestions = [];

                axios.get(`${this.apiIdPath}${movieId}`)
                    .then((response) => {
                        this.movieData = response.data;
                        console.log(this.movieData.Actors)
                    })
                    .catch((err) => {
                        console.log(err);
                    })
            },
            getMoviesSuggestions() {
                 axios.get(`${this.apiSearchPath}${this.title}`)
                        .then((response) => {
                            this.sugestions = response.data.Search;
                        })
                        .catch((err) => {
                            console.log(err);
                        })
            }
        }
    }

</script>

<style lang="sass" scoped>

</style>