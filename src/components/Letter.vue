<template>
  
  <span class="text-2xl text-gray-500" :class="{'animate-pulse text-green-400': active && active_word, 'text-gray-100': correct_word, 'text-red-400': incorrect_word, 'text-xs': letter == '_', 'text-xs text-red-400': letter == '_' && incorrect_word,'text-xs text-gray-600': letter == '_' && correct_word}">{{letter}}</span>

</template>

<script>
export default {
    props: ['letter','active','active_word'],

    methods: {

        register_key_press() {
            document.addEventListener('keydown', e=>{
                if(e.keyCode == 8 && this.active && this.active_word){
                    this.$emit('onBackspace',this.letter)
                }
            })
            document.addEventListener('keypress',e => {
                // console.log(e.keyCode)
                // var input = String.fromCharCode(e.which);
                var input = e.key
                // console.log(e)
                if(e.keyCode == 32){
                    this.checkIfSpaceBar();
                }else{
                    this.checkIfActiveLetter(input);
                }
            })
        },
        checkIfActiveLetter(input){
            if(this.active && this.active_word){
                if(input == this.letter){
                    this.correct_word = true;
                    this.$emit('onCorrectPress',this.letter)
                }else if(input != this.letter){
                    this.incorrect_word = true;
                    this.$emit('onIncorrectPress',this.letter)
                }
            }
        },
        checkIfSpaceBar(input){
            if(this.active && this.active_word){
                if(this.letter == '_'){
                    this.correct_word = true;
                    this.$emit('onCorrectPress',this.letter)
                }else{
                    this.incorrect_word = true;
                    this.$emit('onIncorrectPress',this.letter)
                }
            }
        },

    },

    mounted(){
        this.register_key_press();
    },

    data(){
        return {
            correct_word: false,
            incorrect_word: false,
        }
    },
    watch:{
        active: function(newval, oldval){
            if(newval == true){
                console.log('letter')
                this.correct_word = false;
                this.incorrect_word = false;
            }
        }
    }

}
</script>

<style>

</style>