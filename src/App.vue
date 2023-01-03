<script>
import Logo from "./components/Logo.vue"

let id=0;

export default {
  name: 'App',
  components: {Logo},
  data(){
    return {
      houseType:"ground",
      housePet:"",
      housePrice:0,
      filterPet:"",
      filterType:"",
      items: [
        { id: id++, type:"ground", pet: "cat", price: 20 },
        { id: id++, type:"ground", pet: "dog", price: 40 },
        { id: id++, type:"above", pet: "bird", price: 30 },
      ],
      filterItems:[]
    }
  },
  mounted() {
    this.filterItems=[...this.items]
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
    },
    filterFoo(){
      console.log(this.filterType)
      const hitsByPet = this.items.filter((item) => item.pet.includes(this.filterPet));
      let hitsByType = [];
      if( this.filterType)
        {hitsByType = hitsByPet .filter((item) => item.type===this.filterType);
      }else{
        hitsByType=[...hitsByPet];  
      }
      this.filterItems = [...hitsByType];
    }
  }
};
</script>

<template>
  <div id="app">
    <Logo/>
    <div> 
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
    </div>
    <form class="filters" @submit.prevent="filterFoo">
      <input type="text" v-model="filterPet" placeholder="Find Pet">
      <select name="houseTypeSelect" id="houseTypeSelect" v-model="filterType">
        <option value="">Any</option>
        <option value="ground">ground</option>
        <option value="above">above</option>
      </select>
      <button>Search</button> 
    </form>
    <div>
      
    </div>

    <table>
      <tr>
        <th>Id</th>
        <th>Type</th>
        <th>Pet</th>
        <th>Price</th>
      </tr>
      <tr v-for="item in filterItems" :key="item.id">
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

.filters{
  background-color: yellow;
  padding: 20px;
  margin-top: 10px;
  width: 500px;
  margin: auto;
}

</style>
