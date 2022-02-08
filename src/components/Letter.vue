<template>
  

  <div class="container grid grid-cols-1 w-auto justify-between">
    <span class="text-2xl text-gray-500" :class="{'text-gray-200': letter_array[index] == typed_letter_array[index], 'text-red-400': letter_array[index] != typed_letter_array[index] && typed_letter_array.length > index}">{{letter_array[index]}}</span>
    <span v-if="letter_array[index] != typed_letter_array[index]" class="text-base text-gray-500">{{typed_letter_array[index]}}</span>
  </div>

</template>

<script>
export default {
    props: ['letter_array','typed_letter_array','index'],

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