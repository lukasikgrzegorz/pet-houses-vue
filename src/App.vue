<script>
import Logo from "./components/Logo.vue"

let id=0;

export default {
  name: 'App',
  components: {Logo},
  data(){
    return {
      newItem: '',
      items: [
        { id: id++, type:"ground", pet: "cat", price: 20 },
        { id: id++, type:"ground", pet: "dog", price: 40 },
        { id: id++, type:"above", pet: "bird", price: 30 },
      ]
    }
  }
};
</script>

<template>
  <div id="app">
    <Logo/>
   
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
