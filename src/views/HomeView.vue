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
      <Burger v-for="burger in burgers"
      v-bind:burger = "burger"/>
    </section>

    <section id="order">
      <div>
        <h3>Important information</h3>
        <a> Please provide necessary information to place order</a>
      </div>

      <div>
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
            <label for="street">Street</label><br>
            <input type="text" id="street" v-model="street" placeholder="Street name">
          </p>
          <p>
            <label for="housenumber">House</label><br>
            <input type="number" id="housenumber" v-model="house" placeholder="House number">
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
      </div>

    </section>

    <div>
      <button type="submit" v-on:click = "printOrder">
        <img
          src="https://st2.depositphotos.com/1322515/5340/i/950/depositphotos_53400569-stock-illustration-delivery-scooter.jpg"
          alt="Order" title="Order" style="width: 40px">
        Submit order!
      </button>
    </div>
  </main>
  <hr>
  <footer>
    &copy; ReadyBrugers Inc
  </footer>
  
  
  <div>
    <div id="map" v-on:click="addOrder">
      click here
    </div>
  </div>

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
      street: "",
      house: "",
      payment: "",
      gender: ""
    }
  },
  methods: {

    printOrder: function () {
      console.log(this.fullname, this.email, this.street, this.house, this.payment, this.gender)
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y
        },
        orderItems: ["Beans", "Curry"]
      }
      );
    }
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
header{
	color: white;
}

h1 {
	
	color: gray;
	text-align: center;
	text-transform: uppercase;
}


#burgers {
	padding:30px;
	background-color: black;
	color: white;
	outline: 10px dotted var(--color-of-burgerAlternativ);;
	display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 3rem;
}


#order{
	margin: 20px;
	padding: 40px;
	border: 5px solid gray;
	background-color: aliceblue;
}

button:hover {
	background-color: green;
	cursor:pointer;
}

button{
	margin: 10px;
	height: 50px;
	width: 200px;
}

input[type="radio"]{
	cursor:pointer;
}


#headline img{
	opacity: 0.9;
	width: 100%;
	height: auto;

}

#headline h1{
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
  width: 300px;
  height: 300px;
  background-color: red;
}

</style>