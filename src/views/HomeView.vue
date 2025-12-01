<template>

  <header>
    <div id="headline">
      <h1>Welcome to ReadyBurgers!</h1>
      <img
        src="https://st2.depositphotos.com/1518767/10900/i/600/depositphotos_109003032-stock-photo-friendly-chef-crossed-arms.jpg">
    </div>
    <h3>Select bruger</h3>
    <p>Click burger below to select</p>
  </header>

  <main>

    <section id="burgers">
      <Burger v-for="burger in burgers" v-bind:burger="burger" v-bind:key="burger.name"
        v-on:orderedBurger="addToOrder($event)" />

    </section>

    <section id="order">
      <div>
        <h3>Important information</h3>
        <a> Please provide necessary information to place order</a>
      </div>


      <div id="deliveryInformation">
        <h3>Delivery information:</h3>
        <form>
          <p>
            <label for="fullname">Full name</label><br>
            <input type="text" id="fullname" v-model="fullname" required="required" placeholder="First- and Last name">
          </p>
          <p>
            <label for="email">Email</label><br>
            <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
          </p>
          <p>
            <label for="payment">Select payment option</label><br>
            <select id="payment" v-model="payment">
              <option>Credit card</option>
              <option>Apple Pay</option>
              <option>Bank transaction</option>
              <option>Cash</option>
            </select>
          </p>

          <p>
            <label>Gender</label> <br>
            <input type="radio" id="male" v-model="gender" value="male">
            <label for="male">Male</label> <br>

            <input type="radio" id="female" v-model="gender" value="female">
            <label for="female">Female</label> <br>

            <input type="radio" id="other" v-model="gender" value="other">
            <label for="other">Other</label> <br>

            <input type="radio" id="undiscloed" v-model="gender" value="undiscloed">
            <label for="undiscloed">Undisclosed</label>

          </p>
        </form>

        <h4>Mark delivery location on the map by clicking</h4>
        <div id="map" v-on:click="setLocation">
          <div v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px' }">
            T
          </div>
        </div>

        <button id="orderButton" type="submit" v-on:click="printOrder">
          <img
            src="https://st2.depositphotos.com/1322515/5340/i/950/depositphotos_53400569-stock-illustration-delivery-scooter.jpg"
            alt="Order" title="Order" style="width: 40px">
          Submit order!
        </button>
      </div>

    </section>

  </main>
  <hr>
  <footer>
    &copy; ReadyBrugers Inc
  </footer>




</template>

<script>
import menu from '../assets/menu.json'
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io("localhost:3000");

function MenuItem(name, URL_image, kCal, containsGluten, containsLactose) {
  this.name = name;
  this.img = URL_image;
  this.kCal = kCal;
  this.gluten = containsGluten;
  this.lactose = containsLactose
}



export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fullname: "",
      email: "",
      payment: "",
      gender: "",
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      }
    }
  },
  methods: {


    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      const localX = event.clientX - 10 - offset.x;
      const localY = event.clientY - 10 - offset.y;

      this.location.x = localX;
      this.location.y = localY;
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers);
    },
    printOrder: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y
        },
        burgers: this.orderedBurgers,
        fullname: this.fullname,
        email: this.email,
        payment: this.payment,
        gender: this.gender
      }
      );
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
  }
}

</script>

<style>
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
@import 'https://fonts.googleapis.com/css2?family=Stack+Sans+Notch:wght@200..700&display=swap';

:root {

  --color-of-burgerAlternativ: black;
}

body {
  font-family: Dosis, san-serif;
  font-size: 25pt;
  background-color: brown;
}

header {
  color: white;
}

h1 {

  color: gray;
  text-align: center;
  text-transform: uppercase;
}


#burgers {
  padding: 30px;
  background-color: black;
  color: white;
  outline: 10px dotted var(--color-of-burgerAlternativ);
  ;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 5%;
}


#order {
  margin: 20px;
  padding: 40px;
  border: 5px solid gray;
  background-color: wheat;
}

button:hover {
  background-color: green;
  cursor: pointer;
}

#orderButton {
  margin: 10px;
  height: 50px;
  box-sizing: border-box;
  border-radius: 10px;
  font-size: 40px;
  align-items: center;
}

input[type="radio"] {
  cursor: pointer;
}


#headline img {
  opacity: 0.9;
  width: 100%;
  height: auto;
}

#headline h1 {
  position: absolute;
  font-size: 10vw;
  font-family: "Stack Sans Notch", sans-serif;
  z-index: 1;
  top: 10%;
  left: 50%;
  transform: translate(-50%);
  Color: darkred;
  padding: 20px;
  text-shadow: 1vw 1vw black;
}

#headline {
  position: relative;
}


#map {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  height: 1078px;
  width: 1920px;
  background: url("/img/polacks.jpg");
  cursor: crosshair;
}


#deliveryInformation {
  overflow: auto;
}


#map div {
  position: absolute;
  background: green;
  color: black;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
  font-size: 18px;
}


</style>
