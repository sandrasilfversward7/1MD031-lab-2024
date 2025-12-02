<template>
  <div class="box">    
    <h3> {{ burger.name }} </h3>
      <img v-bind:src="burger.url" style="height: 150px">

      <ul id="allergi"> 
          <li> {{ burger.kCal }} kCal </li>
          <li v-if="burger.gluten"> Innehåller <span class="allergi">gluten</span> </li>
          <li v-if="burger.laktos"> Innehåller <span class="allergi">laktos</span> </li>
          <li v-if="!burger.laktos" style="visibility:hidden"> </li>
          <li v-if="!burger.gluten" style="visibility:hidden"> </li>
      </ul>

    <button v-on:click="addBurger"> + </button>
    <p> Amount Ordered: {{amountOrdered}} </p>
    <button v-on:click="decreaseBurger"> - </button>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },

  data: function(){
    return{
      amountOrdered: 0,
    }
  },
  methods: {
    addBurger: function(){
      this.amountOrdered+=1;
      this.$emit('orderedBurgers', {name: this.burger.name,
                                    amount: this.amountOrdered});
    },

    decreaseBurger: function() {
      if (this.amountOrdered > 0){
        this.amountOrdered-=1;
        this.$emit('orderedBurgers', {name: this.burger.name,
                                    amount: this.amountOrdered});
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box{
    border-radius:5px;
    padding: 10px;
    font-size: 150%;
}
.allergi {
  font-weight: bold;
}
</style>