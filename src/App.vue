<script>
import Logo from "./components/Logo.vue"

const KEY = "Items";
let id=0;

export default {
  name: 'App',
  components: {Logo},
  data(){
    return {
      showModal:false,
      houseType:"ground",
      housePet:"",
      housePrice:null,
      filterPet:"",
      filterType:"",
      sortIsActive:false,
      sortType:{
        id:false,
        type:false,
        pet: false,
        price: false,
      },
      items: [],
      filterItems:[]
    }
  },
  mounted() {
    const savedItems=JSON.parse(localStorage.getItem(KEY));
    if(savedItems){
      this.items=[...savedItems];
      this.filterItems=[...this.items]
      id=this.items.length;
    }
  },
  methods: {
    toogleModal(){
      this.showModal=!this.showModal;
    },
    addNewItem() {
      if(this.houseType&&this.housePrice>0){
        this.items.push({ id: id++, type: this.houseType, pet: this.housePet, price: this.housePrice });
        this.houseType="ground";
        this.housePet="";
        this.housePrice=0;
        this.filterPet="";
        this.filterType="";
        this.sortIsActive=false;
        this.sortType={
          id:false,
          type:false,
          pet: false,
          price: false,
        }
        localStorage.setItem(KEY, JSON.stringify(this.items));
        this.filterItems=[...this.items];
        this.showModal=false;
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
    },
    sortFoo(e){
      const sortType = e.target.id
      switch (sortType) {
      case 'id':
        this.sortType.id=!this.sortType.id;
        if(this.sortType.id){
          this.filterItems.sort((a, b) => b.id - a.id);
        }else{
          this.filterItems.sort((a, b) => a.id - b.id);  
        }
        break;
      case 'price':
        this.sortType.price=!this.sortType.price;
        if(this.sortType.price){
          this.filterItems.sort((a, b) => b.price - a.price);
        }else{
          this.filterItems.sort((a, b) => a.price - b.price);  
        }
        break;
      case 'type':
        this.sortType.type=!this.sortType.type;
        if(this.sortType.type){
          this.filterItems.sort((a, b) => b.type.localeCompare(a.type));
        }else{
          this.filterItems.sort((a, b) => a.type.localeCompare(b.type));  
        }
        break;
      case 'pet':
        this.sortType.pet=!this.sortType.pet;
        if(this.sortType.pet){
          this.filterItems.sort((a, b) => b.pet.localeCompare(a.pet));
        }else{
          this.filterItems.sort((a, b) => a.pet.localeCompare(b.pet));  
        }
        break;
      }  
    },
    resetSort(){
      this.sortIsActive=false;
      this.sortType={
        id:false,
        type:false,
        pet: false,
        price: false,
      },
      this.filterItems=[...this.items]
    }
  }
};
</script>

<template>
  <div id="app">
    <div v-if="showModal" class="backdrop">
      <div class="modal">
        <button class="close-btn" @click="toogleModal">X</button> 
        <h2 class="title">Create new position</h2>
        <form @submit.prevent="addNewItem">
          <div>
            <input type="radio" id="ground" name="house_type" value="ground" v-model="houseType">
            <label for="ground">Ground</label>
            <input type="radio" id="above" name="house_type" value="above" v-model="houseType">
            <label for="above">Above</label>
          </div>
          <input class="input" type="text" v-model="housePet" placeholder="Pet">
          <input class="input" type="number" v-model="housePrice" placeholder="Price PLN">
          <button>Add New Item</button>  
        </form>
      </div>
    </div>
    <Logo/>
    <div class="btn-holder">
      <button @click="toogleModal">Add New Item</button>
      <button @click="resetSort">Reset Filters</button>
    </div>
    <form class="filters" @submit.prevent="filterFoo">
      <input class="input" type="text" v-model="filterPet" placeholder="Find Pet">
      <select class="select" name="houseTypeSelect" id="houseTypeSelect" v-model="filterType">
        <option value="">Any</option>
        <option value="ground">Ground</option>
        <option value="above">Above</option>
      </select>
      <button>Search</button> 
    </form>
    <table v-if="this.items">
      <tr>
        <th @click="sortFoo" id="id">Id</th>
        <th @click="sortFoo" id="pet">Pet</th>
        <th @click="sortFoo" id="type">Type</th>
        <th @click="sortFoo" id="price">Price</th>
      </tr>
      <tr v-for="item in filterItems" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.pet}}</td>
        <td>{{item.type}}</td>
        <td>{{item.price}}</td>
      </tr>
    </table>
  
  </div>
</template>



<style>

*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body{
  background-color: #FEC480;
}

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
  width: 300px;
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

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
  display: flex;
  flex-direction: column;
}

table{
  margin: auto;
}

tr{
  background-color: #FFCD93;
}

td{
  width: 100px;
  text-align: center;
}

th{
  background: brown;
  color: white;
  cursor: pointer;
  padding: 5px;
}

.filters{
  border: 4px dotted black;
  padding: 10px;
  margin-top: 10px;
  margin: auto;
  display: flex;
  gap: 10px;
  justify-content: center;
  align-items: center;
}

.filters .input,
.filters .select{
  margin: auto;
}

.btn-holder{
  display: flex;
  gap: 10px;
  margin: auto;
  margin-top: 10px;
  margin-bottom: 10px;
}

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
