<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order,key) in orders" v-bind:key="'order'+key">
          #{{key}}: Order: {{order.orderedItems}}
          <dd>Contact info: <br>Full name: {{order.details.firstname}} <br>E-mail: {{order.details.email}}<br>Gender: {{order.details.gender}}<br>Payment option: {{order.details.paymentInfo}}<hr></dd>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/observableUniverse.jpg')+ ')' }">
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
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data => //{orders: {...}}
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
    color:white;
    background: rgb(70, 0, 42, 40%);
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  hr {width: 100%; height: 0px; margin-left: auto; margin-right: auto;}
  </style>

