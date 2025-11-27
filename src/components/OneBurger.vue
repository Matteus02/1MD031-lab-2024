<template>
  <div class="burgerAlternativ">
    <h3>{{ burger.name }}</h3>
    <img v-bind:src="burger.img" id="picture"/>
    <ul>
      <h4>About the burger:</h4>
      <li v-if="burger.lactose">Contains Lactose!</li>
      <li v-if="!burger.lactose">Lactose free</li>
      <li v-if="burger.gluten"> Contains gluten!</li>
      <li v-if="!burger.gluten">Gluten free</li>
      <li>Only {{ burger.kCal }} calories</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  }
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#picture{
  width: 300px;
  display:block;
  margin: auto;
}


.burgerAlternativ {
  text-shadow: 2px 2px red;
  padding: 30px;
  border: 5px solid darkred;
}
</style>