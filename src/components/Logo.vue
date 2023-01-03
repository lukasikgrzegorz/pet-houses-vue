<template>
   <div class="logo">Pet Houses</div>
</template>


<script>
export default {
  name: 'Logo'
}
</script>

<style>
.logo{
  font-weight: 700;
  font-size: 40px;
  padding: 15px;
  border: 4px dotted black;
  width: max-content;
  margin: auto;
  transform: rotate(5deg);
  transition: transform 500ms linear;
  margin-bottom: 40px;
}

.logo:hover{
  transform: rotate(0);  
}
</style>
