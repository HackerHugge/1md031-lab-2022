<template>
  <div>
    <header class="imgAndHeader">
      <img src="https://img.freepik.com/premium-vector/pixel-art-landscape-forest-mountains-sunset-minimalistic-8bit-background_360488-281.jpg?w=2000" id="imgHeader" style="height: 300px;" alt="">
      <h1 id="header">Welcome to the internet-burger!</h1>
    </header>

    <header class="secondHeader">
      <h2 id="secondHeader">Making lazy people happy since 2022</h2>
    </header>

    <main>
      <div id="generalPaint">
        Choose between our unique burgers!
      </div>

      <div class="wrapper"> <!-- Creating the burgers with titles, pictures and information about allergens -->
        <Burger v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
      </div>

      <section id="generalPaint">
        <header class="yourInfo">
          <h2>Your information</h2>
        </header>

        <fieldset> <!-- Box for specifying gender -->
          <legend>Specify gender please (We really want to know)</legend>

          <input type="radio" id="man" v-model="gender" value="Man" checked/>
          <label for="man">Man</label>

          <input type="radio" id="kvinna" v-model="gender" value="Woman" />
          <label for="woman">Woman</label>

          <input type="radio" id="nonbin" v-model="gender" value="Non-binary" />
          <label for="nonbin">Non-binary</label>
        </fieldset>

        <form> <!-- Contact information -->
          <p>
            <label for="firstname">Full name</label><br>
            <input type="text" id="firstname" v-model="firstname" placeholder="Full name">
          </p>
          <p>
            <label for="email">E-mail</label><br>
            <input type="email" id="email" v-model="em" placeholder="E-mail">
          </p>

          <!--<p>
            <label for="address">Address</label><br>
            <input type="text" id="address" v-model="ad" placeholder="Street and no.">
          </p> -->

          <p> <!-- Payment information -->
            <label for="recipient">Payment options</label><br>
            <select id="recipient" v-model="rcp">
              <option>Not paying (honestly a fair option)</option>
              <option selected="selected">Good ol' bank (recommended)</option>
              <option>Anonymous scammers Inc.</option>
              <option>All-your-money-belong-to-us-bank</option>
            </select>
          </p>
        </form>
      </section>
      <div id="deliveryText">
        We deliver everywhere in the observable universe. But also we'll probably never reach your destination. <br>
        x marks the (delivery) spot. Good luck!
      </div>
      <div id="map" >
        <div class="map" v-on:click="setLocation" id="dots">
          <div v-bind:style = "{left: this.location.x +'px',
                                 top: this.location.y +'px'}">
            x
          </div>
        </div>
      </div>

      <button type="submit" v-on:click=printInfo id="submit"> <!-- Button for submitting order -->
        <img src="https://c.tenor.com/qRVQPmpDeHUAAAAC/alien-monster-people.gif" alt="Span" style="width: 13px; height: 13px;">
        Put order now!
      </button>
    </main>

    <footer> <!-- Footer regarding company information -->
      <hr>
        &copy;2022 Burgerman Incorporated.
    </footer>
  </div>
</template>

<script>
  import Burger from '../components/OneBurger.vue'
  import io from 'socket.io-client'
  import menu from '../assets/menu.json'

  const newMenu = menu;
  const theMenu=[newMenu[0],newMenu[1],newMenu[2]];

  const socket = io();

 // function menuItem(name,URL,kCal,gluten,lactose){
  //  this.name=name;
  //  this.URL=URL;
  //  this.kCal=kCal;
  //  this.gluten=gluten;
  //  this.lactose=lactose;
  // }

  //const burger1 = new menuItem('Hästburgare', 'https://hips.hearstapps.com/hmg-prod.s3.amazonaws.com/images/copycat-western-bacon-cheeseburger-2-1651789305.jpeg?crop=1.00xw:0.755xh;0,0.166xh&resize=980:*','750 kCal', 'No', 'Yes');
  //const burger2 = new menuItem('Tickle-My-Pickle Deluxe','https://images.dailyhive.com/20190401133001/Screen-Shot-2019-04-01-at-1.27.43-PM.jpg','400','No','No');
  //const burger3 = new menuItem('Den amerikanska (mar)drömmen','https://pbs.twimg.com/media/DTRMZ1XWAAAFoig?format=jpg&name=small','1200','Yes','No');

  // const myBurgers = [burger1,burger2,burger3];
  // console.log(myBurgers);

  export default {
    name: 'HomeView',
    components: {
      Burger
    },
    data: function () {
      return {
        burgers: theMenu,

          firstname: "",
          em: "",
          gender: "",
          rcp: "",
          orderedBurgers: {},
          location: {x: 0,
                     y: 0},
      }
    },
    methods: {

      getOrderNumber: function () {
        return Math.floor(Math.random()*10000)
      },

      addToOrder: function (event) {
        this.orderedBurgers[event.name] = event.amount;
      },
      setLocation: function(event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                      y: event.currentTarget.getBoundingClientRect().top};

        this.location.x = event.clientX - 10 - offset.x;
        this.location.y = event.clientY - 10 - offset.y;
      },

      addOrder: function (event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                      y: event.currentTarget.getBoundingClientRect().top};

        this.location.x = event.clientX - 10 - offset.x;
        this.location.y = event.clientY - 10 - offset.y;
      },
      printInfo: function() {
        socket.emit("addOrder", {
              orderId: this.getOrderNumber(),
              details: {
                       x: this.location.x,
                       y: this.location.y,
                       firstname: this.firstname,
                       email: this.em,
                       gender: this.gender,
                paymentInfo: this.rcp,},

                      orderedItems: this.orderedBurgers,

            },
        );
          console.log(this.firstname, this.gender, this.em, this.rcp, this.location, this.orderedBurgers);
        }
      },      /*printForm: function(){
        var name = document.getElementById("firstname").value;
        console.log(name);
        var email = document.getElementById("email").value;
        console.log(email);
        var address = document.getElementById("address").value;
        console.log(address);
      },*/

    }

</script>

<style>

@import url("https://fonts.googleapis.com/css?family=Lobster");
@import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");

body {
  font-family: "Times New Roman", Lobster, serif;
  font-size: 11pt;
  background-color: rgb(70, 0, 42);
}

a, strong, em {
  color: #0099ff;
}

.imgAndHeader{
  overflow: hidden;
  background:linear-gradient(to bottom, rgba(99, 0, 75, 0) 10%, rgba(74, 0, 63, 0.1) 40%, rgba(116, 0, 116, 0.5) 75%, rgba(41, 0, 36, 0) 100%) repeat scroll 0 0;
}
#imgHeader {
  position: relative;
  width: 100%;
  object-fit: cover;

  opacity: 50%;
  text-align: center;
  color:white;

}
#header {
  font-family: Lobster, serif;
  width: 100%;
  height: auto;
  margin-top: -200px;
  color: white;
  text-align: center;
  position: absolute;
}
h1{
  font-size: 2cm;
}
#secondHeader {
  position: relative;
  font-family: Lobster, serif;
  color: white;
  margin-top: -96px;
  margin-bottom: 40px;
  text-align: center;
}
#generalPaint {
  font-family: monospace;
  color:white;
  text-align: center;
  margin-top: -3px;
  margin-bottom: 10px;
  position: relative;
}
.wrapper {
  font-family: monospace;
  display: grid;
  grid-gap:100px;
  grid-template-columns: 26.5% 26.5% 26.5% ;
  background-color: rgb(43, 0, 41);
  color: rgb(43, 0, 41);
  border: 5px dashed rgba(255, 100, 190, 0.85);
}

#submit {
  margin-left: 540px;
  margin-top: 15px;
}

.box {
  background-color: rgb(43, 0, 41);
  color: #fff;
  padding: 20px;
  width: 100%;
  text-align: center;
}

li{
  text-align: left;
}

#login p {
  width: 80%;
  margin: 1vw auto;
}
p{
  color: white;
}

footer{
  color: white;
}

#lactose {
  color: red;
  text-transform: uppercase;
}
#gluten {
  color: red;
  text-transform: uppercase;
}
#purePower{
  color: rgb(77, 151, 46);
  text-transform: uppercase;
}

button:hover {
  color: rgb(119, 0, 77);
  cursor: pointer;
}

  .map {
    background: url("/public/img/observableUniverse.jpg");
    width: 1920px;
    height: 1038px;
  }

  #map {
    margin: auto;
    width: 900px;
    height: 550px;
    cursor: crosshair;
    overflow: scroll;
    border: 0; padding: 0;
  }
#dots div {
  position: absolute;
  background: white;
  color: rgb(70, 0, 42);
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}
  #map:hover {
    background-color: cornflowerblue;
  }
  #deliveryText{
    text-align: center;
    color: white;
    margin-top: 20px;
    margin-bottom: 10px;
    font-family: monospace;

  }
  .yourInfo {
    margin-top: 40px;
  }
</style>