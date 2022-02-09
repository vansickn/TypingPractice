<template>
  <div v-if="!extra_letters" class="container grid grid-cols-1 w-auto justify-between justify-items-center select-none">
    <span class="text-3xl text-gray-500" :class="{'text-gray-200': letter_array[index] == typed_letter_array[index], 'text-red-400': letter_array[index] != typed_letter_array[index] && typed_letter_array.length > index, 'text-yellow-300 animate-pulse': index == typed_letter_array.length && active_word}">{{letter_array[index]}}</span>
    <span v-if="letter_array[index] != typed_letter_array[index] && typed_letter_array.length <= word_length" class="text-xs text-gray-500">{{typed_letter_array[index]}}</span>
  </div>
  <div v-if="extra_letters" class="w-auto">
      <span class="text-3xl text-red-400">{{typed_letter_array[index]}}</span>
  </div>

</template>

<script>
export default {
    props: ['letter_array','typed_letter_array','index','word_length','typed_letters_length','extra_letters','active_word'],

    methods: {

    },

    mounted(){
    },

    data(){
        return {
        }
    },
    watch:{
        
    }

}
</script>

<style>

</style>