<script>
import Logo from "./components/Logo.vue"

let id=0;

export default {
  name: 'App',
  components: {Logo},
  data(){
    return {
      houseType:"ground",
      housePet:null,
      housePrice:0,
      items: [
        { id: id++, type:"ground", pet: "cat", price: 20 },
        { id: id++, type:"ground", pet: "dog", price: 40 },
        { id: id++, type:"above", pet: "bird", price: 30 },
      ]
    }
  },
  methods: {
    addNewItem() {
      if(this.houseType&&this.housePrice>0){
        this.items.push({ id: id++, type: this.houseType, pet: this.housePet, price: this.housePrice });
        this.houseType="ground";
        this.housePet=null;
        this.housePrice=0;
      }else{
        alert("Fill all fields!")
      }
    }
  }
};
</script>

<template>
  <div id="app">
    <Logo/>
    <form @submit.prevent="addNewItem">
      <div>
        <input type="radio" id="ground" name="house_type" value="ground" v-model="houseType">
        <label for="ground">Ground</label>
        <input type="radio" id="above" name="house_type" value="above" v-model="houseType">
        <label for="above">Above</label>
      </div>
      <input type="text" v-model="housePet" placeholder="Pet">
      <input type="number" v-model="housePrice" placeholder="Price PLN">
      <button>Add New Item</button>  
    </form>
    <table>
      <tr>
        <th>Id</th>
        <th>Type</th>
        <th>Pet</th>
        <th>Price</th>
      </tr>
      <tr v-for="item in items" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.type}}</td>
        <td>{{item.pet}}</td>
        <td>{{item.price}}</td>
      </tr>
    </table>
   
  </div>
</template>



<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

form{
  margin: auto;
  width: 500px;
  padding: 20px;
  background-color: red;
}

table{
  margin: auto;
}

td{
  width: 100px;
  text-align: center;
}

th{
  background: brown;
  color: white;
}

</style>
