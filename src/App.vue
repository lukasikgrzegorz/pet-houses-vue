<script>
import Logo from "./components/Logo.vue"
import Button from "./components/Button.vue"
import Modal from "./components/Modal.vue"
import Filters from "./components/Filters.vue"
import Info from "./components/Info.vue"

const KEY = "Items";
let id=0;

export default {
  name: 'App',
  components: {Logo, Button, Modal, Filters, Info},
  data(){
    return {
      showModal:false,
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
    handleFilters(response){
      const hitsByPet = this.items.filter((item) => item.pet.includes(response.pet));
      let hitsByType = [];
      if( response.type)
        {hitsByType = hitsByPet .filter((item) => item.type===response.type);
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
    handleNewItem(response){
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
    <Modal v-if="showModal" @response="(data)=>handleNewItem(data)" @close="toogleModal"/>
    <Logo/>
    <div class="btn-holder">
      <Button @action="toogleModal" :value="'Add New Item'" />
      <Button @action="resetSort" :value="'Reset Filters'" />
    </div>
    <Filters @response="(data)=>handleFilters(data)"/>
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

.btn-holder{
  display: flex;
  gap: 10px;
  margin: auto;
  margin-top: 10px;
  margin-bottom: 10px;
}

</style>
