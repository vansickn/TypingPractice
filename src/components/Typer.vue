<template>
<div class="container flex flex-row mx-auto pt-20 items-end flex-wrap">
    <Word v-for="w in word_list.length" :key="w" :word="word_list[w-1]" :last_word="w == word_list.length"
     :active="active_word == w-1" :reset="reset" @onEndOfWord="nextWord" @toPreviousWord="previousWord"
     />
</div>
<div class="container flex flex-row mx-auto mt-5 gap-10">
    <h1 class="text-2xl text-gray-100">WPM: {{this.wpm}}</h1>
    <h1 class="text-2xl text-gray-100">Time: {{this.time_elapsed}}</h1>
</div>


</template>

<script>
import Word from '../components/Word.vue';
// import Word from '../components/Word.vue';
export default {
    props: ["word_list","reset"],
    components: {Word},
    methods: {
        nextWord(){
            this.active_word += 1
            if(this.active_word > this.word_list.length-1){
                this.end_time = Date.now()
                this.$emit('onEndSentence')
                this.calculateTimeAndWPM();
            }
        },
        previousWord(){
            if(this.active_word > 0){
                this.active_word -= 1
            }
        },
        calculateTimeAndWPM(){
            this.time_elapsed = (this.end_time - this.start_time)/1000
            this.wpm = (10/this.time_elapsed)*60
            this.start_time = null;
            this.end_time = null;
        }
    },
    mounted() {
        console.log(this.word_list)

        document.addEventListener('keypress', e=>{
            if(this.start_time == null){
                this.start_time = Date.now();
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
        },
    }
}
</script>

<style>

</style>