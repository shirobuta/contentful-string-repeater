<script setup lang="ts">
import {ref, onMounted} from "vue"
import type { FieldAppSDK } from "@contentful/app-sdk";

const props = defineProps<{ sdk: FieldAppSDK }>();

const items = ref<string[]>([""]);

onMounted(() => {
  console.log("String Repeater loaded")
  items.value = props.sdk.field.getValue() || [""];
});

function removeItem(i:int){
  items.value.splice(i, 1);
  saveValue();
}

const saveValue = () => {
  props.sdk.field.setValue([...items.value]).catch((error) => {
    console.error("Error setting field value:", error);
  });
};
</script>

<template>
  <div>
    <div class="items">
      <div v-for="(v, i) in items" :key="i"><input v-model="items[i]" class="input" @input="saveValue()"/><button @click="removeItem(i)">-</button></div>
    </div>
    <button @click="()=>items.push('')">Add entry</button>
    
  </div>
</template>

<style scoped>
.items{
  display:grid;
  gap:0.5em;
  > *{
    display:flex;
    gap:1em;
  }
}
input {
    outline: none;
    box-shadow: rgba(225, 228, 232, 0.2) 0px 2px 0px inset;
    box-sizing: border-box;
    background-color: rgb(255, 255, 255);
    border: 1px solid rgb(207, 217, 224);
    color: rgb(65, 77, 99);
    font-family: "Geist Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    margin: 0px;
    cursor: auto;
    width: 100%;
    z-index: 1;
    border-radius: 6px;
    line-height: 1.25rem;
    font-size: 0.875rem;
    padding: 10px 0.75rem;
    min-height: 40px;
    max-height: 40px;
}

button{
    box-sizing: border-box;
    border: 1px solid rgb(207, 217, 224);
    box-shadow: rgba(25, 37, 50, 0.08) 0px 1px 0px;
    border-radius: 6px;
    cursor: pointer;
    font-family: "Geist Sans", -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    opacity: 1;
    align-items: center;
    font-weight: 500;
    outline: none;
    text-decoration: none;
    margin: 0.5rem 0px 0px;
    color: rgb(17, 27, 43);
    background-color: rgb(255, 255, 255);
    font-size: 0.875rem;
    line-height: 1.25;
    padding: 0.25rem 0.75rem;
    min-height: 2rem;
}
</style>

