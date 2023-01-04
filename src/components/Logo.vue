<script>
export default {
  name: 'Logo'
}
</script>

<template>
  <div class="wrapper">
      <div class="logo">Pet🦊Houses</div>
  </div>
</template>

<style>
.wrapper{
  max-width: 1280px;
  padding: 40px 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: auto;
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
