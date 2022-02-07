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
        async fetchRandomWords(){
            let response = await fetch('https://random-word-api.herokuapp.com/word?number=10');
            return response.json()
        },
        randomWordsReset(){
            this.fetchRandomWords().then((words)=>{
                this.word_list_updated = this.massageWordList(words);
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