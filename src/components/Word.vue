<template>

    <div class="container flex flex-row w-auto">
        <div class="container flex flex-col">
            <div class="container flex flex-row w-auto">
                <Letter v-for="n in word.length" :key="n" :letter_array="word.split('')" :typed_letter_array="typed_letters" :index="n-1" :word_length="word.length" :typed_letters_length="typed_letters.length" :extra_letters="false" :active_word="active"/>
                <!-- if extra -->
                <Letter v-for="n in typed_letters.length" :key="n" :typed_letter_array="typed_letters" :index="word.length + n-1" :extra_letters="true"/>
            </div>
            <div v-if="active" class="w-auto h-1 bg-yellow-300 rounded-lg transition duration-200"></div>
        </div>
    </div>


  
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
                this.handleBackspace(e);
            }
        },
        handleKeyPress(e){
            e.preventDefault();
            var input = e.key
            console.log(input)
            if(e.keyCode == 32){ //space bar
            }else{
                this.checkActive(input)
            }
        },
        handleBackspace(e){
            if(this.typed_letters.length == 0){
                this.$emit("toPreviousWord")
            }else{
                if(e.altKey == true){
                    this.typed_letters = [];
                }else{
                    this.typed_letters.pop()
                }
            }

        },
        checkActive(input){
            if(this.active){
                this.typed_letters.push(input);
                console.log(this.typed_letters.length)
                console.log(this.typed_letters)
                this.checkCorrectness();
            }
        },
        checkCorrectness(){
            console.log(JSON.stringify(this.typed_letters))
            if(JSON.stringify(this.typed_letters) == JSON.stringify(this.word.split(''))){
                this.$emit('onCorrectWord')
            }else{
                this.$emit('onIncorrectWord')
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