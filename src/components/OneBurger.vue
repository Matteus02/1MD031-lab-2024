<template>
  <div class="burgerAlternativ">

    <div id="header">
      <h3>{{ burger.name }}</h3>
      <img v-bind:src="burger.img" id="picture" />
    </div>
    <ul id="information">
      <h4>About the burger:</h4>
      <li v-if="burger.lactose">Contains Lactose!</li>
      <li v-if="!burger.lactose">Lactose free</li>
      <li v-if="burger.gluten"> Contains gluten!</li>
      <li v-if="!burger.gluten">Gluten free</li>
      <li>Only {{ burger.kCal }} calories</li>
    </ul>

    <h4 id="orderCounter">
      <button id="decreaseButton" v-on:click="removeBurger">-</button>
      Amount: {{ amountOrdered }}
      <button id="increaseButton" v-on:click="addBruger">+</button>
    </h4>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },


  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {

    addBruger: function () {
      if (this.amountOrdered >= 20) {
        alert("We do not accept online orders larger than 20 burgers. Please call if you want to order more than");
      }
      else {
        this.amountOrdered++;
        this.$emit('orderedBurger', {
          name: this.burger.name,
          amount: this.amountOrdered
        })
      }

    },

    removeBurger: function () {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('orderedBurger', {
          name: this.burger.name,
          amount: this.amountOrdered
        })
      }
      else { this.amountOrdered = 0; }
    }
  }
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#picture {
  width: 300px;
  display: block;
  margin: auto;
}


.burgerAlternativ {
  text-shadow: 2px 2px red;
  padding: 30px;
  border: 5px solid darkred;
  display: grid;
  grid-template-rows: 3fr;

}

#header {
  grid-row: 1;
}

#information {
  grid-row: 2;
}


#orderCounter {
  background-color: wheat;
  border: 5px solid darkred;
  border-radius: 25px;
  display: flex;
  justify-content: center;
  padding: 10px;
  grid-row: 3;

}


#decreaseButton {
  border: 5px black;
  width: 40px;
  height: 40px;
  box-sizing: border-box;
  border-radius: 10px;
  background-color: red;
  font-size: 30px;
  margin: 10px;

}

#increaseButton {
  border: 5px black;
  width: 40px;
  height: 40px;
  box-sizing: border-box;
  border-radius: 10px;
  background-color: green;
  font-size: 30px;
  margin: 10px;

}
</style>
