<script>
import Logo from "./components/Logo.vue"
import Button from "./components/Button.vue"
import Modal from "./components/Modal.vue"
import Info from "./components/Info.vue"

const KEY = "Items";
let id=0;

export default {
  name: 'App',
  components: {Logo, Button, Modal, Info},
  data(){
    return {
      showModal:false,
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
      this.filterPet="";
      this.filterType="";
      this.sortType={
        id:false,
        type:false,
        pet: false,
        price: false,
      },
      this.filterItems=[...this.items]
    },
    handleResponde(response){
      this.toogleModal();
      this.items.push({ id: id++, type: response.type, pet: response.pet, price: response.price });
      localStorage.setItem(KEY, JSON.stringify(this.items));
      this.filterItems=[...this.items];
      this.resetSort();
    }
  }
};
</script>

<template>
  <div id="app">
    <Modal v-if="showModal" @response="(data)=>handleResponde(data)" @close="toogleModal"/>
    <Logo/>
    <div class="btn-holder">
      <Button @action="toogleModal" :value="'Add New Item'" />
      <Button @action="resetSort" :value="'Reset Filters'" />
    </div>
    <form class="filters" @submit.prevent="filterFoo">
      <input class="input" type="text" v-model="filterPet" placeholder="Find Pet">
      <select class="select" name="houseTypeSelect" id="houseTypeSelect" v-model="filterType">
        <option value="">Any</option>
        <option value="ground">Ground</option>
        <option value="above">Above</option>
      </select>
      <Button :value="'Search'"/>
    </form>
    <table v-if="this.filterItems.length > 0">
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
    <Info v-if="this.filterItems.length === 0" />
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

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
  display: flex;
  flex-direction: column;
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

table{
  margin: auto;
}

tr{
  background-color: #FFCD93;
}

td{
  width: 125px;
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
  width:100%;
  max-width: 500px;
  flex-wrap: wrap;
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

</style>
