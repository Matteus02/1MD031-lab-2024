<template>
  <div id="orders">
    <div id="orderList">
      <div v-for="(order, key) in orders" v-bind:key="'order' + key" id="orderItem">
        #{{ key }}:

        <div id="burgerOrder" v-for="(amount, name) in order.burgers" :key="name">
          <div v-if="amount > 0">{{ name }}: {{ amount }}.</div>
        </div>

        <div id="orderInformation">{{ order.fullname }}, {{ order.email }}, {{ order.payment }}, {{ order.gender }}
        </div>
      </div>
      <button v-on:click="clearQueue">Clear Queue</button>
    </div>
    <div id="dots">
      <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px' }"
        v-bind:key="'dots' + key">
        {{ key }}
      </div>
    </div>
  </div>
</template>

<script>
import io from 'socket.io-client'
const socket = io("localhost:3000");

export default {
  name: 'DispatcherView',
  data: function () {
    return {
      orders: null,
    }
  },
  created: function () {
    socket.on('currentQueue', data =>
      this.orders = data.orders);
  },
  methods: {
    clearQueue: function () {
      socket.emit('clearQueue');
    },
    changeStatus: function (orderId) {
      socket.emit('changeStatus', { orderId: orderId, status: "Annan status" });

    }
  }
}
</script>

<style>
#orderList {
  top: 1em;
  left: 1em;
  position: absolute;
  z-index: 2;
  color: black;
  background: rgba(255, 255, 255, 0.5);
  padding: 1em;
}

#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  width: 1920px;
  height: 1078px;
  cursor: crosshair;
  background-image: url('/img/polacks.jpg');
}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}

#burgerOrder {
  font-weight: bold;
  font-size: 20px
}

#orderInformation {
  font-size: 20px;
  font-style: italic;

}

#orderItem{
  margin-top: 20px;
  border-bottom: 2px solid black;
}
</style>