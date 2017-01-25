<template>
        <form class="pos-relative">
                <label class="label col-w" for="title-one">Pick first title</label>
                <input class="input w-12" type="text" name="title-one" v-model="title" @keydown="getMoviesSuggestions()">
                <div class="pos-absolute w-12 top-100">
                    <ul class="list">
                        <li class="list-item" v-for="sugestion in sugestions" @click="pickTitle(sugestion.Title)">
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
                apiPath: 'http://www.omdbapi.com/?s='
            }
        },
        methods: {
            pickTitle(choosenTitle) {
                this.title = choosenTitle;
                this.sugestions = [];
            },
            getMoviesSuggestions() {
                 axios.get(`${this.apiPath}${this.title}`)
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