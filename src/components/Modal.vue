<script>
 export default{
  name: "Modal",
  emits: ['response'],
  data(){
    return {
      houseType:"ground",
      housePet:"",
      housePrice:null,
    }
  },
 }   
</script>

<template>
    <div class="backdrop">
      <div class="modal">
        <button class="close-btn" @click="$emit('close')">X</button> 
        <h2 class="title">Create new position</h2>
        <form @submit.prevent="$emit('response', {type:houseType, pet:housePet, price:housePrice})">
          <div>
            <input type="radio" id="ground" name="house_type" value="ground" v-model="houseType">
            <label for="ground">Ground</label>
            <input type="radio" id="above" name="house_type" value="above" v-model="houseType">
            <label for="above">Above</label>
          </div>
          <input class="input" type="text" v-model="housePet" placeholder="Pet" required>
          <input class="input" type="number" v-model="housePrice" placeholder="Price PLN" required>
          <button>Add New Item</button>  
        </form>
      </div>
    </div>
</template>

<style>

.backdrop{
  position: absolute;
  width: 100vw;
  height: 100vh;
  background: #07070781;
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
}

.title{
  width: 100%;
  text-align: center;
  font-size: 18px;
}

.modal{
  width: 100%;
  max-width: 300px;
  background-color: #ffffff;
  position: relative;
  padding: 30px 20px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  border-radius: 15px;
}

.modal form{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.input,
.select{
  font-size: 16px;
  margin-bottom: 10px;
  padding: 5px;
  border-radius: 5px;
  outline:none;
  border: 2px solid black;
}

.close-btn{
  position: absolute;
  top: 15px;
  right: 15px;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover{
  transform: scale(1.1);
}

</style>
