<template>
  <div v-if="!extra_letters" class="container grid grid-cols-1 w-auto justify-between">
    <span class="text-2xl text-gray-500" :class="{'text-gray-200': letter_array[index] == typed_letter_array[index], 'text-red-400': letter_array[index] != typed_letter_array[index] && typed_letter_array.length > index}">{{letter_array[index]}}</span>
    <span v-if="letter_array[index] != typed_letter_array[index] && typed_letter_array.length <= word_length" class="text-base text-gray-500">{{typed_letter_array[index]}}</span>
  </div>
  <div v-if="extra_letters" class="w-auto">
      <span class="text-2xl text-red-400">{{typed_letter_array[index]}}</span>
  </div>

</template>

<script>
export default {
    props: ['letter_array','typed_letter_array','index','word_length','typed_letters_length','extra_letters'],

    methods: {

    },

    mounted(){
        // console.log(this.letter_array[index])
    },

    data(){
        return {
            // correct_word: false,
            // incorrect_word: false,
        }
    },
    watch:{
        
    }

}
</script>

<style>

</style>