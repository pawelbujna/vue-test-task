<template>
    <div class="p-l-10 p-r-10">
        <form class="p-t-10 p-l-10 p-r-10 p-b-5 bg-col-primary">
            
            <InputWrap 
            :inputName="movieOne" 
            @moviePicked="setRightMovie"  
            @inputIsEmpty="ifInputIsEmpty">
            </InputWrap>
            
            <InputWrap 
            :inputName="movieTwo" 
            @moviePicked="setRightMovie"  
            @inputIsEmpty="ifInputIsEmpty">
            </InputWrap>

        </form>

        <Result v-if="initResult" :items="sameActors"></Result>

        <PastItems></PastItems>

    </div>
</template>

<script>
    import InputWrap from './../inputWrap/InputWrap.vue'
    import Result from './../result/Result.vue'
    import PastItems from './../pastItems/PastItems.vue'
    import axios from 'axios';
    import _ from 'lodash';

    export default {
        components: {
            InputWrap,
            Result,
            PastItems
        },
        data() {
            return {
                movieOne: 'movie1',
                movieTwo: 'movie2',
                movieOneData: [],
                movieTwoData: [],
                movieOneActors: [],
                movieTwoActors: [],
                sameActors: [],
                initResult: false
            }
        },
        methods: {
            ifInputIsEmpty(inputName) {

                if (inputName == this.movieOne) {
                    this.movieOneActors = [];
                } else if (inputName == this.movieTwo) {
                    this.movieTwoActors = [];
                }

                this.sameActors = [];
            },
            clearData() {
                this.movieOneActors = [];
                this.movieTwoActors = [];
            },
            setRightMovie(movie) {

                if ( movie.name == this.movieOne ) {
                    this.movieOneData = [];
                    this.movieOneData.push(movie);
                } else if (movie.name == this.movieTwo){
                    this.movieTwoData = [];
                    this.movieTwoData.push(movie)
                }

                this.setSameActors();
            },
            setSameActors() {
                if (!(this.movieOneData.length && this.movieTwoData.length)) {
                    this.sameActors = [];
                    return;
                }

                this.clearData();

                this.movieOneActors = this.movieOneData[0].data.Actors.split(', ')
                this.movieTwoActors = this.movieTwoData[0].data.Actors.split(', ')

                this.sameActors =  _.intersection(this.movieOneActors, this.movieTwoActors);
                this.initResult = true;
            }
        }
    }

</script>