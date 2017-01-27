<template>
    <div class="m-b-10 pos-relative">

        <label class="label col-w">Pick title</label>
        <input class="input w-12" 
            type="text" 
            v-model="title" 
            @keyup="getMoviesSuggestions()" 
            @change="checkIfItsEmpty()"
            @focus="isFocus()"
            @blur="isBlur()"
            autocomplete="off">

        <div class="pos-absolute w-12 z-3 top-100" v-if="sugestions && sugestions.length > 0 && isActive">
            <ul class="list">
                <li class="list-item" v-for="sugestion in sugestions" @click="pickTitle(sugestion.imdbID, sugestion.Title)">
                    {{sugestion.Title}}
                </li>
            </ul>
        </div>

    </div>
</template>

<script>
    import axios from 'axios'
    import firebase from 'firebase'
    import _ from 'lodash'

    let firebaseRef = firebase.database().ref()

    export default {
        props: [ 'inputName' ],
        data() {
            return {
                title: '',
                sugestions: [],
                movieData: {},
                finalData: {},
                isVisible: false,
                isActive: true,
                apiSearchPath: 'https://www.omdbapi.com/?s=',
                apiIdPath: 'https://www.omdbapi.com/?i='
            }
        },
        methods: {
            isFocus() {
                this.isActive = true;
            },
            isBlur() {
                setTimeout(() => {
                    this.isActive = false;
                }, 200)
            },
            checkIfItsEmpty() {
                if (this.title.length == 0) {
                    this.$emit('inputIsEmpty', this.inputName)
                }
            },
            pickTitle(movieId, title) {
                this.title = title;
                this.sugestions = [];

                axios.get(`${this.apiIdPath}${movieId}`)
                    .then((response) => {
                        this.movieData = response.data;
                        this.finalData = {
                            name: this.inputName,
                            data: this.movieData
                        }

                        this.$emit('moviePicked', this.finalData);
                    })
                    .catch((err) => {
                        console.log(err);
                    })

                firebaseRef.child('movies').push({title: this.title});
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