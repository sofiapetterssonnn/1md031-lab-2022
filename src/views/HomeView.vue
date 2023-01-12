<template>

<div>

<header> 
    <img src="https://www.multivu.com/players/English/8246355-chilis-boss-burger/image/ChilisHero_1526904726393-null-HR.jpg" style = "width:100%; height: 350px; opacity: 0.5;">
    <h1 style="position: absolute; right: 240px; margin-top: -200px; font-size: 40pt;">WELCOME TO PETTERSSON'S BURGERS</h1>
</header>

<section id = selectBurger>  
        <h1>Select burger</h1>
        <p> This is were you execute burger selection</p>
        <div class ="wrapper">
          <Burger v-for="burger in burgers"
          v-bind:burger="burger"
          v-bind:key="burger.name"
          v-bind:src="burger.img"
          v-bind:key1="burger.ingredients"
          v-on:orderedBurger="addToOrder($event)"/>
        </div>
            

</section>
    
<section id = customerInfo style = "clear:left"> 
    <h2>Customerinformation</h2>
    <p>This is where you provide necessary information</p>  
    <h3>Deliveryinformation</h3>
        <p>
            <label for="First- and lastname">Full name</label><br>
            <input type="text" id="First- and lastname" v-model="fn" required="required" placeholder="First- and lastname">
        </p>
        <p>
            <label for="email">E-mail</label><br>
            <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        </p>
        <!-- <p>
            <label for="Street name">Street</label><br>
            <input type="text" id="Street name" v-model="sn" placeholder="Street name">
        </p> -->
        <!-- <p>
            <label for="House number">House</label><br>
            <input type="number" id="House number" v-model="hn" placeholder="House number">
        </p> -->

        <p>
            <label for="Payment">Payment</label><br>
            <select id="recipient" v-model="rcp">
                <option>Credit card</option>
                <option>Swish</option>
                <option>Invoice</option>
                <option>Klarna</option>
            </select>
        </p>

          <label for="gender">Gender</label> <br> 
          <input type="radio"  id="male"  value="male" v-model="gender" checked> 
          <label for="male">Male</label> <br>
          <input type="radio"  id="female" value="female"  v-model="gender">
          <label for="female">Female</label> <br>
          <input type="radio" id="non-binary" value="non-binary" v-model="gender"> 
          <label for="non-binary">Non-binary</label>  <br>
          <br>

 <p>Place indicate point of delivery</p>
  <div id="mapscroller">
  <div id="map" v-on:click="setLocation">
  <div id = "dots">
    <div v-bind:style="{left: location.x + 'px', top: location.y + 'px'}">
      T
    </div>
    </div>
  </div>
</div> <br>

        <button v-on:click="submitted" class="button">
            <img src= "https://static.vecteezy.com/system/resources/previews/001/193/924/non_2x/car-png.png"
            width="30"
            height="20" >
                Place my order!
        </button>
    </section>
           
        <hr>
        <footer> © 2022 Pettersson's Burgers AB</footer>




</div>

</template>


<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();


/*function MenuItem(name, img, kCal, lactose, gluten) {
  this.name = name;
  this.url = img;
  this.kCal = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
  
}

let burger1 = new MenuItem("Soffis Burger", "https://cdn.cnn.com/cnnnext/dam/assets/220428140436-04-classic-american-hamburgers-full-169.jpg", 650, true, true)
let burger2 = new MenuItem("Beast Burger", "https://www.tastingtable.com/img/gallery/heres-how-hamburgers-got-their-name/intro-1653066580.jpg", 700, true, true)
let burger3 = new MenuItem("Cheese Burger", "https://www.tastingtable.com/img/gallery/steakburger-vs-hamburger-whats-the-biggest-difference/l-intro-1657643502.jpg", 650, true, true)*/

const burgerlista = menu;
console.log(burgerlista);


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      amountOrdered: 0,
      orderedBurgers: {},
      fn: "",
      em: "",
      gender: 'male',
      rcp: 'Credit card',
      

      location: {
        x: 0,
        y: 0
      },

    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      
    this.location.x = event.clientX - 10 - offset.x;
    this.location.y = event.clientY - 10 - offset.y; 

  },

  addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;
  console.log(event.name);
  },
  submitted: function (){
  socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y },
                                orderItems: this.orderedBurgers,
                                form:{fn: this.fn, em: this.em, rcp: this.rcp, gender: this.gender}
                                }
                                         
                 );                         
  },

  setLocation: function(event) {
    var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      
    this.location.x = event.clientX - 10 - offset.x;
    this.location.y = event.clientY - 10 - offset.y;

  }
  }
}
</script>

<style>

@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
@import url('https://fonts.googleapis.com/css2?family=Lexend+Deca:wght@200;400&family=Raleway:wght@200;700&display=swap');

body {
    font-family: 'Raleway', sans-serif;
    font-size: 13pt;
 }

 .wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 33% 33% 33%;
    background-color: #877764;
    color: white;
    justify-items: center;
}

 .ingredients {
    color: white;
 }

 #ingredient {
    font-weight: bold;
    color: white;
 }

 #selectBurger {
    background-color:#877764;
    color: white;
    margin: 10px 10px 5px 10px;
    padding: 5px 10px;
    border: solid black 2px;
 }

 #customerInfo {
    margin: 5px 10px 5px 10px;
    padding: 5px 10px;
    border: solid black 2px;
 }

 button:hover {
    cursor: pointer;
 }

 #dots {
  position: relative;
  margin: 0;
  padding: 0;
  background: url("/Users/sofiapettersson/Desktop/Programmering/Granssnitt/1md031-lab-2022/public/img/polacks.jpg");
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

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/Users/sofiapettersson/Desktop/Programmering/Granssnitt/1md031-lab-2022/public/img/polacks.jpg");
    cursor:pointer;
  }
  #mapscroller {
    width: 1380px;
    height: 300px;
    overflow: scroll;
  }

</style>
  