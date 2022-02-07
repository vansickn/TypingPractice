<template>
{{active_word}}
<div class="container flex flex-row mx-auto pt-20 items-end flex-wrap">
    <Word v-for="w in word_list.length" :key="w" :word="word_list[w-1]" :last_word="w == word_list.length"
     :active="active_word == w-1" :reset="reset" @onEndOfWord="nextWord" @toPreviousWord="previousWord"
     />
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
                this.$emit('onEndSentence')
            }
        },
        previousWord(){
            if(this.active_word > 0){
                this.active_word -= 1
            }
        }
    },
    mounted() {
        console.log(this.word_list)
    },
    data() {
        return {
            active_word: 0,
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
        },
    }
}
</script>

<style>

</style>