<template>
  <header>
    <img v-bind:src="headImg"> 
    <h1> Miracle Burgers</h1> 
  </header>

  <section class="burgerSection">
    <h2>Please choose a burger</h2>
    <h3>If you don't choose one below you won't be able to reap the benefits</h3>
    <div class="wrapper">
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
    </div>
  </section>
  <section id="formSection" >
                <h2>Please tell us about yourself..</h2>
                <h3>.. by filling out this form</h3>
                <form>
                    <p>
                        <label for="fullname">Full name</label><br>
                        <input type="text" id="fullname" v-model="fn" required="required" placeholder="First- and Last name">
                        <br>
                    </p>
                    <p>
                        <label for="email">E-mail</label><br>
                        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                    </p>
                    <p>
                        <label for="streetname">Street name</label><br>
                        <input type="text" id="streetname"  v-model="sn" placeholder="Street name">

                    </p>
                    <p>
                        <label for="housenumber">House number</label><br>
                        <input type="number" id="housenumber" v-model="hn" placeholder="House number">

                    </p>
                    <p>
                        <label for="payment">Payment method</label><br>
                        <select id="payment" v-model="pay">
                            <option selected="select">Credit card </option>
                            <option>Debit card </option>
                            <option>Swoosh </option>
                            <option>Cash</option>
                        </select>

                    </p>
                        <h3>Gender</h3>
                        <p>
                            <input type="radio" id="male" value="Male" v-model="gn" checked="checked">
                            <label for="male">Male</label>
                        </p>
                        <p>
                            <input type="radio" id="female" value="Female" v-model="gn">
                            <label for="female">Female</label>
                        </p>
                        <p>
                            <input type="radio" id="nonbinary" value="Nonbinary" v-model="gn">
                            <label for="nonbinary">Non-binary</label>
                        </p>
                        <p>
                            <input type="radio" id="notspecified" value="Notspecified" v-model="gn">
                            <label for="notspecified">I don't want to specify</label>
                        </p>
                        
                </form>
                
                <section >
                  
                  <h2>Pic location on map</h2>
                  <div id="mapWrap" >
                    <div id="mapImg" v-on:click="setLocation">
                      <div id="position" v-bind:style="{ left: location.x + 'px', top: location.y + 'px' } "> T </div>
                    </div>
                  </div>
                </section>
                <!-- -->

                <button id="orderButton" v-on:click="submiting" type="submit" >
                  <img src="https://media.istockphoto.com/vectors/burger-cartoon-vector-id1195987636?s=612x612" alt="Burger icon" width="15px" height="15px" >
                  Place order
                </button>

            </section>
            
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'
const socket = io();


const burgerArray = menu;

export default {

  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burgerArray,
      headImg: 'https://3dzip.org/wp-content/uploads/2019/01/3D-Model-Interior-Restaurant-221-Free-Download-3.jpg',
      fn: '', 
      em: '', 
      pay:'', 
      gn: 'Male',
      orderedBurgers: {},
      location: {x: 0, y:0}
    }
    
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    
    setLocation: function(event){
      console.log(event);
      const offset = event.target.getBoundingClientRect();
      this.location.x = event.clientX-offset.left-10;
      this.location.y = event.clientY-offset.top-10;
    },
     addToOrder: function (event) {
      this.orderedBurgers[event.burgerId] = event.amount;
      console.log(this.orderedBurgers);
    },
    submiting: function() {
      
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           namn: this.fn,
                                           email: this.em,
                                           pay: this.pay,
                                           gender: this.gn},
                                orderItems: this.orderedBurgers
                             }
                  );
    
    },
  }
}

</script>

<style>

body {
    font-family: Arial, Helvetica, sans-serif;
    display: grid;
    grid-template-columns: 1500px;
}

header {
    height: 300px;
    overflow: hidden;
    margin-left: 1rem;
    margin-right: 1rem;
    grid-row: 1;
}
h1{ 
    position: absolute;
    top: 1em;
    left: 2.4em; 
    font-size: 4em;
    -webkit-text-stroke: 1px black;
    color: #5A0C00;
    font-family: 'Mochiy Pop One', sans-serif, Arial, Helvetica, sans-serif;

}

header img{
    opacity: 0.65;
    position: relative;
    top: -150%;
}

.wrapper{
  display: grid;
  grid-gap: 20px;
  grid-template-columns: auto auto auto;
}

.burgerSection{
  background-color: black;
  color: white;
  border: 2px dashed white;
  margin-left: 1rem;
  margin-right: 1rem;
  padding: 2rem;
  grid-row: 2;

}
#formSection{
    clear: left; border: 2px dashed black;
    margin-left: 1rem;
    margin-right: 1rem;
    padding: 2rem;
    grid-row: 3;

}
#mapWrap{
  height: 600px;
  width: 1000px;
  overflow: scroll;
  position: relative;
}
#mapImg{
  background-image: url(../../public/img/polacks.jpg);
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
}

#position{
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}

#orderButton{
  margin-top: 2rem;
  font-weight: bold;
  font-size: 1em;
}

</style>
