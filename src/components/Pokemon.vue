<template>
  <div class="pokemon-container">
    <img :src="img" alt="No se pudo">
    <p>{{text}}</p>


  </div>
</template>

<script>
export default {
    props:{
        img: String,
        text:String
    }

}
</script>

<style>

img{
    margin: 20px;
}



</style>