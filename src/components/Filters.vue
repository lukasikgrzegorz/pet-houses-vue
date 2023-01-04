<script>
import Button from "./Button.vue"
  export default{
    name: "Filters",
    components:{Button},
    data(){
      return{
        filterPet:"",
        filterType:""
      }
    }
  }
</script>

<template>
 <form class="filters" @submit.prevent="$emit('response', {pet:filterPet, type:filterType})">
      <input type="text" v-model="filterPet" placeholder="Find Pet">
      <select name="houseTypeSelect" id="houseTypeSelect" v-model="filterType">
        <option value="">Any</option>
        <option value="ground">Ground</option>
        <option value="above">Above</option>
      </select>
      <Button :value="'Search'"/>
    </form>
</template>

<style scoped>

.filters{
  border: 4px dotted black;
  padding: 10px;
  margin-top: 10px;
  margin: auto;
  display: flex;
  gap: 10px;
  justify-content: center;
  align-items: center;
  width:100%;
  max-width: 500px;
  flex-wrap: wrap;
}

input,
select{
  font-size: 16px;
  margin-bottom: 10px;
  padding: 5px;
  border-radius: 5px;
  outline:none;
  border: 2px solid black;
  margin: auto;
}



</style>