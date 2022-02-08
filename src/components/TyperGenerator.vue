<template>
    
    <div class="container flex flex-row mx-auto">
        <div class="container flex flex-col">
            <h1 class="text-gray-200 mx-auto ml-20 pt-10">Choose words from: </h1>
            <div class="container flex flex-row gap-5">
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-200 items-center justify-center rounded-lg select-none" :class="{'bg-green-400': difficulty_array.includes('easy')}" @click="adjustDifficulty('easy')">
                    Easy
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-200 items-center justify-center rounded-lg select-none" :class="{'bg-yellow-400': difficulty_array.includes('medium')}" @click="adjustDifficulty('medium')">
                    Medium
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-200 items-center justify-center rounded-lg select-none" :class="{'bg-red-400': difficulty_array.includes('hard')}" @click="adjustDifficulty('hard')">
                    Hard
                </div>
            </div>
        </div>
        <div class="container flex flex-col">
            <h1 class="text-gray-200 ml-20 pt-10">Number of words: </h1>
            <div class="container flex flex-row gap-5">
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-200 items-center justify-center rounded-lg select-none" :class="{'bg-green-400': num_words == 10}" @click="changeNumWords(10)">
                    10
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-200 items-center justify-center rounded-lg select-none" :class="{'bg-yellow-400': num_words == 25}" @click="changeNumWords(25)">
                    25
                </div>
                <div class="conatainer flex flex-row cursor-pointer w-20 h-10 bg-gray-200 items-center justify-center rounded-lg select-none" :class="{'bg-red-400': num_words == 40}" @click="changeNumWords(40)">
                    40
                </div>
            </div>

        </div>
    </div>

    <Typer :word_list="word_list_updated" @onEndSentence="resetTyper" :reset="reset" :number_of_words="num_words"/>
    <!-- <div v-show="is_loading">
        <svg class="w-10 h-10 animate-spin" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M11.49 3.17c-.38-1.56-2.6-1.56-2.98 0a1.532 1.532 0 01-2.286.948c-1.372-.836-2.942.734-2.106 2.106.54.886.061 2.042-.947 2.287-1.561.379-1.561 2.6 0 2.978a1.532 1.532 0 01.947 2.287c-.836 1.372.734 2.942 2.106 2.106a1.532 1.532 0 012.287.947c.379 1.561 2.6 1.561 2.978 0a1.533 1.533 0 012.287-.947c1.372.836 2.942-.734 2.106-2.106a1.533 1.533 0 01.947-2.287c1.561-.379 1.561-2.6 0-2.978a1.532 1.532 0 01-.947-2.287c.836-1.372-.734-2.942-2.106-2.106a1.532 1.532 0 01-2.287-.947zM10 13a3 3 0 100-6 3 3 0 000 6z" clip-rule="evenodd"></path></svg>
    </div> -->

    <Dictionary v-if="random_words" :difficulty="difficulty_array" :number_of_words="num_words" :reset="reset" @onChosenWords="pickedWordsFromDictionary"/>
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
            is_loading: true,
            session: 0,
            reset: false,
            difficulty_array: ['easy'],
            num_words: 10,
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