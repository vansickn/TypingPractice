<template>
<div class="container flex flex-row mx-auto pt-14 pb-20 justify-center gap-2 flex-wrap mt-10 select-none">
    <input type="text" autocorrect="off" autocapitalize="off" name="" id="" class="absolute w-10/12 opacity-0 cursor-default">
    <Word v-for="w in word_list.length" :key="w" :word="word_list[w-1]" :last_word="w == word_list.length"
     :active="active_word == w-1" :reset="reset" @onEndOfWord="nextWord" @toPreviousWord="previousWord"
     @onCorrectWord="alterCorrectness(true)" @onIncorrectWord="alterCorrectness(false)"
     />
</div>

</template>

<script>
// import { loadConfigFromFile } from 'vite';
import Word from '../components/Word.vue';
// import Word from '../components/Word.vue';
export default {
    props: ["word_list","reset","number_of_words"],
    components: {Word},
    methods: {
        nextWord(){
            this.end_word_time = Date.now();
            this.saveWordTime();
            this.active_word += 1
            this.start_word_time = Date.now();
            if(this.active_word > this.word_list.length-1){
                this.end_time = Date.now()
                this.calculateTimeAndWPM();
                this.$emit('onEndSentence',this.word_wpm_array,this.correctness_array, this.wpm, this.time_elapsed, this.correct_words);
            }
        },
        previousWord(){
            if(this.active_word > 0){
                this.active_word -= 1
            }else{
                this.active_word = 0;
            }
        },
        calculateTimeAndWPM(){
            this.time_elapsed = (this.end_time - this.start_time)/1000
            this.start_time = null;
            this.end_time = null;
            this.correct_words = 0;
            for (let i = 0; i < this.correctness_array.length; i++) {
                if(this.correctness_array[i] == true){
                    this.correct_words += 1
                }
            }
            this.wpm = Math.round((this.correct_words/this.time_elapsed)*60)

        },
        alterCorrectness(bool){
            if(bool){
                this.correctness_array[this.active_word] = true;
                if(this.active_word == this.word_list.length-1){
                    // ends and resets
                    this.nextWord()
                }
            }else if(!bool){
                this.correctness_array[this.active_word] = false;
            }
        },
        saveWordTime(){
            var word_time_elapsed = (this.end_word_time - this.start_word_time)/1000;
            this.word_wpm_array[this.active_word] = Math.round((1/word_time_elapsed)*60);
            this.start_word_time = null;
            this.end_word_time = null;
        }
    },
    mounted() {
        console.log(this.word_list)

        document.addEventListener('keypress', e=>{
            if(this.start_time == null){
                this.start_time = Date.now();
            }
            if(this.start_word_time == null){
                this.start_word_time = Date.now();
            }
            if(e.keyCode == 32){
                // space bar
                this.nextWord();
            }
        });
    },
    data() {
        return {
            active_word: 0,
            start_time: null,
            end_time: null,
            time: null,
            time_elapsed: null,
            wpm: null,
            correctness_array: [],
            correct_words: 0,

            word_wpm_array: [],
            start_word_time: null,
            end_word_time: null,

        };
    },
    components: { Word },
    watch: {
        word_list: function(newv){
            console.log(newv)
            if(newv.length != 0){
                console.log("not null")
            }
        },
        reset: function(newv){
            this.active_word = 0
            this.start_time = null;
            this.end_time = null;
            this.correctness_array = [];
            this.start_word_time = null;
            this.end_word_time = null;
        },
    }
}
</script>

<style>

</style>