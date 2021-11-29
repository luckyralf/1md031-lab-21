<template>
<div >
  <div>
    
      <h4>{{burger.name}}</h4>
      <img id="imgId" v-bind:src="burger.image"> 
      <li> {{burger.kCal}} kCal </li>
      <span v-if="burger.lactose == true"> <li> Contains <span class="allergies" > lactose </span></li></span>
      <span v-if="burger.lactose == false"> <li><span class="allergies" > Lactose </span> free</li></span>
      <span v-if="burger.gluten == true"> <li> Contains <span class="allergies" > gluten </span></li></span>
      <span v-if="burger.gluten == false"> <li><span class="allergies" > Gluten </span> free</li></span>
      <p>Amount: 
        <button v-on:click='removeBurger'>–</button>
        {{amountOrdered}}
        <button v-on:click='addBurger'>+</button>
      </p> 
      
    </div>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data:function(){
    return {
      amountOrdered: 0,
    }
  },
  
  methods: {

    addBurger: function(){
        this.amountOrdered += 1;
        console.log(this.burger.burgerId);
        this.$emit('orderedBurger', { burgerId : this.burger.burgerId,
                                      amount: this.amountOrdered
                                    }
                  );
      },
    removeBurger: function(){
        this.amountOrdered -= 1;
        this.$emit('orderedBurger', { burgerId : this.burger.burgerId,
                                      amount: this.amountOrdered
                                    }
                  );
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#imgId{
  height: 300px;
  width: auto;
}
.allergies{
  font-weight: bold;
}

</style>
