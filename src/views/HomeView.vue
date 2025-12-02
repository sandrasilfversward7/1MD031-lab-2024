<template>
  <div>
    <header>
      <img src="/img/restu.jpg">
      <h1>VälkomWelmen till Smarriga BurgareOnline</h1>
    </header>

    <main>
    <section id="Burgerinfo">
      <h2> Vilken burgare är du sugen på?</h2>
          <p> Här får du välja helt fritt vilken burgare du vill ha</p>
          <div class="wrapper">   
            <Burger v-for="burger in burgers" 
                    v-bind:burger="burger" 
                    v-bind:key="burger.name"
                    v-on:orderedBurgers="addToOrder($event)"/>
            </div>
    </section>

    <section id="Orderinfo"> 
      <div>
        <h2> Kundinformation</h2>
          <p> Här ska du få fylla i information om dig själv</p>
          <h3> Leverans information</h3>
                   
          <p> 
            <label for ="Förochefternamn"> Namn </label><br>
            <input type="text" id="Förochefternamn" v-model="fullName" required="required" placeholder="För- och efternamn">
          </p> 
                
          <p>
            <label for ="Email"> Email </label><br>
            <input type="email" id="email" v-model="email" required="required" placeholder="Email-adress">
          </p>
      </div>

      <div> 
        <h3> Betalningsalternativ </h3>
          <p>
            <label for="betalningsmetod">  </label>
            <select id="betalningsmetod" v-model="betalningsmetod">
              <option> Betala med kort </option>
              <option> Betala med swish </option>
              <option> Betala med klarna </option>
              <option> Betala med faktura </option>
              </select>
            </p>
        </div>

        <div id="contain-map">
          <div id="map" v-on:click="setLocation">
        </div>

        <div id="target" v-bind:style="{left: location.x + 'px', top:location.y +'px'}">
        T
        </div>
      </div>
      
      <div>
          <h3> Personlig information</h3>
            <p>
              <input type="radio" id="gender" v-model="gender" checked="checked" value ="Vill inte ange"/>
                  <label for="gender"> Vill inte ange </label>
            </p>
           
            <p>
              <input type="radio" id="gender" v-model="gender" value ="Kvinna"/>
              <label for="gender"> Kvinna </label>
            </p>
            
            <p>
              <input type="radio" id="gender" v-model="gender" value ="icke binär"/>
              <label for="gender"> Icke binär </label>
            </p>

            <p>
              <input type="radio" id="gender" v-model="gender" value ="Man"/>
              <label for="gender"> Man </label>
            </p>
        </div>          
    </section>  

      <button type="submit" v-on:click="order">
        <img src="/img/button.jpg" style="height: 50px">
         Placera order!
      </button>

  </main>

  <hr> <footer>
    <p> Copyright</p>
  </footer>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'

const socket = io("localhost:3000");

function MenuItem(name, url, kCal, gluten, lactose){
  this.name = name; 
  this.url = url; 
  this.kCal = kCal;
  this.gluten = gluten;
  this.laktos = lactose;
}

export default {
  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: menu,
      fullName: "",
      email:"",
      betalningsmetod: "",
      gender: "",
      orderedBurgers: {},
      location: {x:0, y:0}
    }
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    
    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

    this.location.x = event.clientX - 10 - offset.x;
    this.location.y = event.clientY - 10 - offset.y;
    },

    addToOrder: function (event){
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers);
    },

    order: function(){
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y},
                               
                                orderItems: this.orderedBurgers,
                                customer: {
                                  name: this.fullName,
                                  email: this.email,
                                  gender: this.gender,
                                  betalningsmetod:this.betalningsmetod
                                }
                              });
      console.log("fullName: ",this.fullName);
      console.log("email: ",this.email);
      console.log("payment: ",this.betalningsmetod);
      console.log("gender: ",this.gender);
    }
  }
}
</script>

<style>
  #contain-map{
    position: relative;
    overflow:scroll;
  }

  #map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
  }

  #target{
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;

  }

    @import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
      body {
      font-family: "Times New Roman", serif;
    font-size: 20pt;
}


h1 {
    font-family: 'Agbalumo';
    font-size: 36pt;
}
main, header, footer, nav ul {
    max-width: 100rem;
    margin: 0 auto;

}
main {
    background-color: white;
}

/* nav ul li {
    display: inline-block;
    background-color: grey;
    padding: 1em;
    margin: 1em;
} */


header {
    width: 100%;
    height: 100px;
    position:relative;
    overflow: hidden; 
}

header img{
    position: absolute;
    width: 100%;
    height: auto;
    opacity: 0.5;  
}

header h1 {
    width: 100%;
    height: auto;
    margin: 0 auto;
    text-align: center;
    color: rgb(202, 0, 0);
    opacity: 1;
    position:relative;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

.allergi{
    font-weight:bold
}

#Burgerinfo{
    background-color:black;
    color: white;
    border: 3px dashed white;
    margin: 0px
}

#Burgerinfo div{
    margin: 10x;
    padding: 10px;
}

#Orderinfo{
    background-color:white;
    border: 3px dashed black;
    margin: 10px;
    color: black;
}

button:hover{
    background-color: blue;
    cursor:pointer;
}

button{
    margin-top: 20px;
    padding: 10px;
}

section{
    margin: 40px 40px;
    padding: 10px
}

.wrapper{
    display: grid;
    grid-gap:20px;
    grid-template-columns: 33% 33% 33%;    
}

.box{
    border-radius:5px;
    padding: 10px;
    font-size: 150%;
}
</style>