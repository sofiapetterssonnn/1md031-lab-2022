<template>

<div id = "burger" class="burger" style="float:left;">
  <h3>{{burger.name}}</h3>
  <img :src="burger.url" alt="" style="margin-right: 15px; height: 160px; width: 260px">
  <h4>{{"Ingredients"}}</h4>

 
  <ul>
    <li> {{ burger.calories}} kCal </li>
    <li v-if="burger.lactose"> Contains <span class="ingredient"> lactose </span></li>
    <li v-if="burger.gluten"> Contains <span class="ingredient"> gluten </span></li>
  </ul>

  <div>
  <button v-on:click="decreaseBurger" class="numberOfBurgers">
    -
  </button>

  <button v-on:click="increaseBurger" class="numberOfBurgers">
    +
  </button>
  <p>Amount: {{ amountOrdered }} </p>
  </div>

</div>

</template>
  

<script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
  },

  data: function() {
    return {
      amountOrdered: 0,
    }
  },

  methods: {
    increaseBurger: function() {
      this.amountOrdered++
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      }
    );

  },
  decreaseBurger: function() {
    this.amountOrdered--
      this.$emit('orderedBurger', {
      name: this.burger.name,
      amount: this.amountOrdered
      }
    );
  }
  }    
  }

  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

   .wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 33% 33% 33%;
    background-color: #877764;
    color: white;
    justify-items: center;
}
.box {

}

.a {
  grid-column: 1;
  grid-row:1;
  
;

}
.ingredient{
  font-weight: bold;
}
  
  </style>