<template> <!-- Burgers' single element details-->

    <div>
      <div class="box">            <h3>{{ burger.name }} {{ burger.kCal }}</h3>

        <img v-bind:src="burger.URL" alt="Span" style="width: 310px; height: 200px;">

        <ul>
          <li>{{burger.info}}</li>
          <li v-if="burger.gluten" id="gluten"> {{"Contains gluten"}}</li>
          <li v-if="burger.lactose" id="lactose"> {{"Contains lactose"}}</li>
          <li v-if="burger.purePower" id="purePower"> {{"Contains extraterrestrial power"}}</li>
        </ul>

        <button @click="addBurger" id="changeOrder">
          More!
        </button>

        {{amountOrdered}} added to basket

        <button @click="removeBurger">
          Nah!
        </button>


      </div>
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

      addBurger: function () {
        this.amountOrdered += 1;
        this.$emit('orderedBurger', { name:   this.burger.name,
          amount: this.amountOrdered}
        );
      },

      removeBurger: function () {
        if(this.amountOrdered > 0){
          this.amountOrdered -= 1;
        }

        this.$emit('orderedBurger', { name:   this.burger.name,
          amount: this.amountOrdered}
        );
      },

    },



  }

  </script>
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  #changeOrder {
    text-align:center;
  }

  </style>
  