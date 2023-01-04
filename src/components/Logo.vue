<template>
  <div class="container">
      <div class="logo">Pet🦊Houses</div>
  </div>
</template>


<script>
export default {
  name: 'Logo'
}
</script>

<style>
.container{
  max-width: 1280px;
  padding: 40px 15px;
}
.logo{
  font-weight: 700;
  font-size: 40px;
  padding: 15px 15px 15px;
  border: 4px dotted black;
  width: max-content;
  transform: rotate(5deg);
  transition: transform 500ms linear;
  margin: auto;
}

.logo:hover{
  transform: rotate(0);  
}
</style>
