<template>
  <div id="orders">
    <div id="orderList">
      <div v-for="(order, key) in orders" v-bind:key="'order'+key">
        <span id="orderNrAndBurgers" v-if="order.orderItems.MB > 0"> Order: {{ key }}: 
          <span  v-if="order.orderItems.MB > 0">MB:  {{order.orderItems.MB}}, </span>
          <span  v-if="order.orderItems.AMB > 0"> Alost MB: {{order.orderItems.AMB}},</span>
          <span  v-if="order.orderItems.VMB > 0"> Very MB: {{order.orderItems.VMB}}</span> 
        </span> <br>
        Info: {{order.details.namn}}, {{order.details.email}}, {{order.details.pay}}, {{order.details.gender}},
        
      </div>
      <button v-on:click="clearQueue">Clear Queue</button>
    </div>
    <div id="dots">
        <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
          {{ key }}
        </div>
    </div>
  </div>
</template>
<script>
import io from 'socket.io-client'
const socket = io();

export default {
  name: 'Dispatcher',
  data: function () {
    return {
      orders: null
    }
  },
  created: function () {
    socket.on('currentQueue', data =>
      this.orders = data.orders);
  },
  methods: {
    clearQueue: function () {
      socket.emit('clearQueue');
    }
  }
}
</script>
<style>

#orderList {
  top:1em;
  left:1em;
  position: absolute;
  z-index: 2;
  color:black;
  background: rgba(255,255,255, 0.5);
  padding: 1em;
}
#orderNrAndBurgers{
  font-size: 1.2em;
  font-weight: bold;
}
#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
</style>
