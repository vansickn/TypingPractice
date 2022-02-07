<template>
    <Typer :word_list="word_list_updated" @onEndSentence="resetTyper" :reset="reset"/>
    <div v-show="is_loading">
        <svg class="w-10 h-10 animate-spin" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M11.49 3.17c-.38-1.56-2.6-1.56-2.98 0a1.532 1.532 0 01-2.286.948c-1.372-.836-2.942.734-2.106 2.106.54.886.061 2.042-.947 2.287-1.561.379-1.561 2.6 0 2.978a1.532 1.532 0 01.947 2.287c-.836 1.372.734 2.942 2.106 2.106a1.532 1.532 0 012.287.947c.379 1.561 2.6 1.561 2.978 0a1.533 1.533 0 012.287-.947c1.372.836 2.942-.734 2.106-2.106a1.533 1.533 0 01.947-2.287c1.561-.379 1.561-2.6 0-2.978a1.532 1.532 0 01-.947-2.287c.836-1.372-.734-2.942-2.106-2.106a1.532 1.532 0 01-2.287-.947zM10 13a3 3 0 100-6 3 3 0 000 6z" clip-rule="evenodd"></path></svg>
    </div>

    <Dictionary v-if="random_words" :difficulty="'easy'" :number_of_words="10" :reset="reset" @onChosenWords="pickedWordsFromDictionary"/>
</template>

<script>
import Typer from '../components/Typer.vue';
import Dictionary from '../components/Dictionary.vue';
export default {
    props: ['word_list','random_words'],
    components: {Typer,Dictionary},
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
        // massageRandomWordList(arr){
        //     var updated_arr = []
        //     for (let i = 0; i < arr.length; i++) {
        //         updated_arr.push(arr[i].word);
        //         if(i < arr.length-1){
        //             updated_arr.push("_");
        //         }
        //     }
        //     return updated_arr;
        // },
        // async fetchRandomWords(){
        //     let response = await fetch('https://random-word-api.herokuapp.com/word?number=10');
        //     // let response = await fetch('https://api.wordnik.com/v4/words.json/randomWords?hasDictionaryDef=true&maxCorpusCount=-1&minDictionaryCount=1&maxDictionaryCount=-1&minLength=1dis&maxLength=8&limit=10&api_key=f0n0z9b5ibxt0mz6qcuuqb5dtjxb5vmb9yfng54uj1utghiye');
        //     // console.log(response.json())
        //     return response.json()
        // },
        // randomWordsReset(){
        //     // https://developer.wordnik.com/docs#!/words/getRandomWords
        //     this.fetchRandomWords().then((words)=>{
        //         // https://api.wordnik.com/v4/words.json/randomWords?hasDictionaryDef=true&maxCorpusCount=-1&minDictionaryCount=1&maxDictionaryCount=-1&minLength=2&maxLength=8&limit=10&api_key=f0n0z9b5ibxt0mz6qcuuqb5dtjxb5vmb9yfng54uj1utghiye
        //         this.word_list_updated = this.massageWordList(words);
        //     })
        // },
        resetTyper(){
            this.reset = true;
            // this.randomWordsReset();
            this.session += 1;
            this.is_loading = true;
            this.$nextTick(()=>{
                this.reset = false;
                this.is_loading = false;
            })
        },
        pickedWordsFromDictionary(words){
            this.word_list_updated = this.massageWordList(words)
        },
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
        document.addEventListener('keydown', e=>{
            if(e.keyCode == 9){
                e.preventDefault();
                this.resetTyper();
            }
        })
        if(this.random_words){
            // this.fetchDictionaryWords();
        }

    }
}
</script>

<style>

</style>