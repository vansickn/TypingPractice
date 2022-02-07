<template>
   <Letter v-for="n in word.length" :key="n" :letter="word.charAt(n-1)" :active="checkActive(n-1)" :active_word="active"
    @onCorrectPress="onCorrectLetterPress" @onIncorrectPress="onIncorrectLetterPress" @onBackspace="onBackspaceEvent"
    />
   <!-- <span class="text-3xl text-gray-400" :class="{'text-gray-400 text-base':word == '_'}">{{word}}</span> -->

  
</template>

<script>
import Letter from '../components/Letter.vue';
export default {
    props: ['word','last_word','active'],
    components: {Letter},

    methods: {
        onCorrectLetterPress(letter){
            // need to set timeout for 1ms in order to switch active
            setTimeout(()=>{
                this.active_letter += 1;
                this.checkEndWord();
            }, 1)
        },
        onIncorrectLetterPress(letter){
            setTimeout(()=>{
                this.active_letter += 1;
                this.checkEndWord();
            }, 1)
        },
        onBackspaceEvent(letter){
            setTimeout(()=> {
                this.previousLetter()
            })
        },
        checkActive(n){
            if(n == this.active_letter){
                return true
            }else{
                return false
            }
        },
        checkEndWord(){
            if(this.active_letter >= this.word.length){
                this.$emit('onEndOfWord');
                // this.active_letter -= 1;
            }
        },
        previousLetter(){
            console.log('hello')
            if(this.active_letter == 0){
                console.log("previous word")
                this.$emit('toPreviousWord')
            }else{
                this.active_letter -= 1;
            }
        }
    },

    mounted(){
        // this.register_key_press();
        if(this.active){
            console.log(this.word)
        }
    },
    data(){
        return {
            active_letter: 0
        }
    },
    watch: {
        active: function(newv,oldv){
            console.log(newv)
            console.log(oldv)
            console.log(this.active_letter)
            if(oldv == false && newv == true){
                if(this.active_letter == this.word.length){
                    this.active_letter = this.word.length-1;
                    console.log('hello')
                }
            }
        }
    }

}
</script>

<style>

</style>