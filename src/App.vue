<script setup>
import SuggestionInput from './components/SuggestionInput.vue';
import { ref, reactive } from 'vue';
const items = reactive([
  'Bob',
  'Bill',
  'Bonnie',
  'Beluga',
  'Conrad',
  'Colin',
  'indigo',
  'illinois',
]);
const enteredText = ref('');
</script>

<template>
  <SuggestionInput v-model:enteredText="enteredText" :items="items"/>
</template>
