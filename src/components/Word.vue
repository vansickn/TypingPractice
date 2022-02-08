<template>
   <!-- <Letter v-for="n in word.length" :key="n" :letter="word.charAt(n-1)" :active="checkActive(n-1)" :active_word="active" :reset="reset"
    @onCorrectPress="onCorrectLetterPress" @onIncorrectPress="onIncorrectLetterPress" @onBackspace="onBackspaceEvent"
    /> -->

    <!-- <span class="text-2xl text-gray-500">{{word}}</span>
    {{typed_letters}} -->
    <div class="container flex flex-row w-auto">
        <Letter v-for="n in word.length" :key="n" :letter_array="word.split('')" :typed_letter_array="typed_letters" :index="n-1"/>
    </div>
   <!-- <span class="text-3xl text-gray-400" :class="{'text-gray-400 text-base':word == '_'}">{{word}}</span> -->

  
</template>

<script>
import Letter from '../components/Letter.vue';
export default {
    props: ['word','last_word','active','reset'],
    components: {Letter},

    methods: {
        register_key_press() {
            if(this.active){
                document.addEventListener('keydown', this.handleKeyDown);
                document.addEventListener('keypress', this.handleKeyPress);
            }
        },
        handleKeyDown(e){
            if(e.keyCode == 8 && this.active){
                // this.$emit('onBackspace',this.letter)
                this.handleBackspace();
            }
        },
        handleKeyPress(e){
            var input = e.key
            console.log(input)
            if(e.keyCode == 32){ //space bar
               console.log("spacebar!") 
            }else{
                this.checkActive(input)
                // this.typed_letters.push(String.fromCharCode(e.keyCode));
                // console.log(String.fromCharCode(e.keyCode));
            }
        },
        handleBackspace(){
            if(this.typed_letters.length > 0){
                this.typed_letters.pop()
            }else{
                this.$emit("toPreviousWord");
            }
        },
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
        checkActive(input){
            if(this.active){
                this.typed_letters.push(input);
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
        this.register_key_press();
    },
    data(){
        return {
            active_letter: 0,
            typed_letters: [],
        }
    },
    watch: {
        active: function(newv,oldv){
            if(newv == true){
                console.log("registering new keypress")
                this.register_key_press();
            }
        },
        reset: function(newv){
            if(newv){
                console.log("reset word")
                this.active_letter = 0;
                this.typed_letters = [];
                document.removeEventListener('keydown',this.handleKeyDown);
                document.removeEventListener('keypress',this.handleKeyPress);
                this.register_key_press();
            }
        }
    }

}
</script>

<style>

</style>