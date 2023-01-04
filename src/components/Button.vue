<script>
 export default{
  name: "Button",
  props: {
    value: String
  }
 }   
</script>

<template>
  <button @click="$emit('action')">{{value}}</button>
</template>

<style scoped>
  button{
    font: inherit;
    width: max-content;
    margin: auto;
    font-size: 14px;
    padding: 10px;
    border-color: transparent;
    cursor: pointer;
    border-radius: 5px;
    background-color: #000000;
    color: #ffffff;
    transition: transform 350ms linear;
  }

  button:hover{
    transform: scale(1.03);
  }
</style>