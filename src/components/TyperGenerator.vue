<template>
    <Typer v-show="!is_loading" :word_list="word_list_updated" @onEndSentence="endedSentence" :reset="reset"/>
</template>

<script>
import Typer from '../components/Typer.vue'
export default {
    props: ['word_list','random_words'],
    components: {Typer},
    methods: {
        massageWordList(arr){
            var updated_arr = []
            for (let i = 0; i < arr.length; i++) {
                updated_arr.push(arr[i]);
                if(i < arr.length-1){
                    updated_arr.push("_");
                }
            }
            return updated_arr;
        },
        massageRandomWordList(arr){
            var updated_arr = []
            for (let i = 0; i < arr.length; i++) {
                updated_arr.push(arr[i].word);
                if(i < arr.length-1){
                    updated_arr.push("_");Kuf
                }
            }
            return updated_arr;
        },
        async fetchRandomWords(){
            // let response = await fetch('https://random-word-api.herokuapp.com/word?number=10');
            let response = await fetch('https://api.wordnik.com/v4/words.json/randomWords?hasDictionaryDef=true&maxCorpusCount=-1&minDictionaryCount=1&maxDictionaryCount=-1&minLength=1&maxLength=8&limit=10&api_key=f0n0z9b5ibxt0mz6qcuuqb5dtjxb5vmb9yfng54uj1utghiye');
            // console.log(response.json())
            return response.json()
        },
        randomWordsReset(){
            // https://developer.wordnik.com/docs#!/words/getRandomWords
            this.fetchRandomWords().then((words)=>{
                // https://api.wordnik.com/v4/words.json/randomWords?hasDictionaryDef=true&maxCorpusCount=-1&minDictionaryCount=1&maxDictionaryCount=-1&minLength=2&maxLength=8&limit=10&api_key=f0n0z9b5ibxt0mz6qcuuqb5dtjxb5vmb9yfng54uj1utghiye

                this.word_list_updated = this.massageRandomWordList(words);
                this.is_fetching = false;
            })
        },
        endedSentence(){
            this.reset = true;
            this.randomWordsReset();
            this.session += 1;
            this.is_loading = true;
            this.$nextTick(()=>{
                this.reset = false;
                this.is_loading = false;
            })
        }
    },
    data(){
        return {
            word_list_updated: [],
            is_loading: true,
            session: 0,
            reset: false,
        }
    },
    mounted(){
        if(this.random_words){
            this.randomWordsReset();
        }

    }
}
</script>

<style>

</style>