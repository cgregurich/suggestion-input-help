<script setup>
import { ref, reactive, computed } from 'vue';


const props = defineProps({
  items: Array,
  enteredText: String,
});

const emit = defineEmits(['update:enteredText'])


const isFocused = ref(false);

const suggestions = computed(() => {
  if (props.enteredText=== '') return;
  return props.items.filter(item => fuzzyMatch(props.enteredText, item));
});

function fuzzyMatch(query, string) {
  query = query.toLowerCase();
  string = string.toLowerCase();
  let queryPointer = 0;
  for (let c of string) {
    if (queryPointer >= query.length) break;
    if (c === query[queryPointer]) queryPointer++;
  }
  if (queryPointer >= query.length) return true;
  return false;
}

const inputField = ref();

function suggestionClicked(suggestion) {
  console.log('emitting ', suggestion);
  emit('update:enteredText', suggestion);
  inputField.value.focus();
}
  
</script>

<template>
<div class="search-input">
  isFocused: {{ isFocused }}
  <input type="text" ref="inputField" @focus="isFocused = true" @blur="isFocused = false" v-model="enteredText" @input="$emit('update:enteredText', $event.target.value)">
  <div v-show="suggestions?.length > 0 && isFocused" class="suggestions">
    <div @click="suggestionClicked(suggestion)" class="suggestion" v-for="(suggestion, index) in suggestions" :key="index">{{ suggestion }}</div>
  </div>
</div>
</template>

<style scoped>
.suggestion-input {
  position: relative;
  display: flex;
  flex-direction: column;
}
.suggestions {
  position: absolute;
  width: 100%;
  background-color: white;
  border: 1px solid black;
}
.suggestion:hover {
  background-color: blue;
  color: white;
  cursor: default;
}
</style>