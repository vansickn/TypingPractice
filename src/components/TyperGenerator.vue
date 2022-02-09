<template>
    
    <div class="container flex flex-row justify-start sm:gap-10 sm:ml-20 flex-wrap">
        <div class="container flex flex-col items-center sm:w-64">
            <h1 class="text-gray-400 pt-2 sm:pt-10">Choose words from: </h1>
            <div class="container flex flex-row gap-2 sm:gap-5 mt-1 justify-center">
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-500 items-center justify-center rounded-lg select-none" :class="{'bg-green-500': difficulty_array.includes('easy')}" @click="adjustDifficulty('easy')">
                    Easy
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-500 items-center justify-center rounded-lg select-none" :class="{'bg-yellow-500': difficulty_array.includes('medium')}" @click="adjustDifficulty('medium')">
                    Medium
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-500 items-center justify-center rounded-lg select-none" :class="{'bg-red-500': difficulty_array.includes('hard')}" @click="adjustDifficulty('hard')">
                    Hard
                </div>
            </div>
        </div>
        <div class="container flex flex-col items-center sm:w-64">
            <h1 class="text-gray-400 pt-2 sm:pt-10">Number of words: </h1>
            <div class="container flex flex-row gap-2 sm:gap-5 mt-1 justify-center">
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-500 items-center justify-center rounded-lg select-none" :class="{'bg-green-500': num_words == 10}" @click="changeNumWords(10)">
                    10
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-500 items-center justify-center rounded-lg select-none" :class="{'bg-yellow-500': num_words == 25}" @click="changeNumWords(25)">
                    25
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-500 items-center justify-center rounded-lg select-none" :class="{'bg-red-500': num_words == 40}" @click="changeNumWords(40)">
                    40
                </div>
            </div>

        </div>
    </div>

    <Typer :word_list="word_list_updated" @onEndSentence="resetTyper" :reset="reset" :number_of_words="num_words"/>
    <!-- <div v-show="is_loading">
        <svg class="w-10 h-10 animate-spin" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M11.49 3.17c-.38-1.56-2.6-1.56-2.98 0a1.532 1.532 0 01-2.286.948c-1.372-.836-2.942.734-2.106 2.106.54.886.061 2.042-.947 2.287-1.561.379-1.561 2.6 0 2.978a1.532 1.532 0 01.947 2.287c-.836 1.372.734 2.942 2.106 2.106a1.532 1.532 0 012.287.947c.379 1.561 2.6 1.561 2.978 0a1.533 1.533 0 012.287-.947c1.372.836 2.942-.734 2.106-2.106a1.533 1.533 0 01.947-2.287c1.561-.379 1.561-2.6 0-2.978a1.532 1.532 0 01-.947-2.287c.836-1.372-.734-2.942-2.106-2.106a1.532 1.532 0 01-2.287-.947zM10 13a3 3 0 100-6 3 3 0 000 6z" clip-rule="evenodd"></path></svg>
    </div> -->

    <div class="mx-auto container flex flex-row justify-center gap-2 cursor-pointer select-none w-40 mb-10" @click="resetTyper">
        <svg class="w-6 h-6 text-gray-500" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4 2a1 1 0 011 1v2.101a7.002 7.002 0 0111.601 2.566 1 1 0 11-1.885.666A5.002 5.002 0 005.999 7H9a1 1 0 010 2H4a1 1 0 01-1-1V3a1 1 0 011-1zm.008 9.057a1 1 0 011.276.61A5.002 5.002 0 0014.001 13H11a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0v-2.101a7.002 7.002 0 01-11.601-2.566 1 1 0 01.61-1.276z" clip-rule="evenodd"></path></svg>
        <span class="text-base text-gray-500"> click or press TAB</span>
    </div>


    <Graph class="w-scren mx-auto" :word_list="word_list_updated" :wpm_array="wpm_array" :correctness_array="correct_array" :generate="reset"/>


    <Dictionary v-if="random_words" :difficulty="difficulty_array" :number_of_words="num_words" :reset="reset" @onChosenWords="pickedWordsFromDictionary"/>



</template>
<script>
import Typer from '../components/Typer.vue';
import Dictionary from '../components/Dictionary.vue';
import Graph from '../components/Graph.vue';

export default {
    props: ['word_list','random_words'],
    components: { Typer, Dictionary, Graph},
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
        resetTyper(wpm_array, correct_array){
            this.wpm_array = wpm_array;
            this.correct_array = correct_array;
            this.reset = true;
            // this.randomWordsReset();
            this.session += 1;
            this.$nextTick(()=>{
                this.reset = false;
            })
        },
        pickedWordsFromDictionary(words){
            // this.word_list_updated = this.massageWordList(words)
            this.word_list_updated = words;
        },
        adjustDifficulty(diff){
            const i = this.difficulty_array.indexOf(diff)
            if(this.difficulty_array.includes(diff)){
                this.difficulty_array.splice(i,1) //remove index of diff
            }else{
                this.difficulty_array.push(diff)
            }
            if(this.difficulty_array.length == 0){
                this.difficulty_array.push('easy');
            }
            this.resetTyper();
        },
        changeNumWords(n){
            this.num_words = n;
            this.resetTyper();
        }
    },
    data(){
        return {
            word_list_updated: [],
            session: 0,
            reset: false,
            difficulty_array: ['easy'],
            num_words: 10,

            wpm_array: null,
            correct_array: null,
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