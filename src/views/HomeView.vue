<template>
  <div class="home">
    <img class="inline" alt="Vue logo" src="../assets/logo.png">
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    Number of articles in your cart : {{ userCart.length }}
    <div class="grid grid-cols-3 gap-4">
      <div :class="[cartButton && divID == index? 'inline border border-blue-600 p-2' : 'inline border border-black p-2']" v-bind:key="product.id" 
            v-for="(product, index) in productList" v-on:click="addCartButton(index)">
        {{ product.title }} 
        <br>
        <p> <img class="max-h-32 max-w-[25%] inline" v-bind:src="product.image"> </p>
        $ {{ product.price }} 
        <br>
        <button class="rounded-lg bg-blue-600 text-white ml-2 p-2 hover:bg-red-500" v-on:click="addToCart(product)" v-if="cartButton && divID == index">Add To Cart</button>
        <button class="rounded-lg bg-blue-600 text-white ml-2 p-2 hover:bg-red-500" v-on:click="removeToCart(product)" v-if="cartButton && divID == index">Remove from Cart</button>
      </div>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'HomeView',
  components: {
    //HelloWorld
  },
  data()
  {
    return {
      productList: [],
      cartButton: false,
      divID: null,

      userCart: [],
      //userCartProductTemplate: {article:null, quantity:null}
    }
  },
  methods:{
    addCartButton(addCartButtonIndex){
      this.cartButton = true;
      this.divID = addCartButtonIndex;
    },
    delCartButton(){
      // method not used for now
      this.cartButton = false;
    },
    addToCart(chosenProduct){
      this.userCart.push(chosenProduct);
      console.log(this.userCart);
    },
    findIndexOf(param){
      return this.userCart.indexOf(param);
    },
    removeToCart(articleToRemove){
      if (this.userCart.includes(articleToRemove)){
        this.userCart.splice(this.findIndexOf(articleToRemove), 1);
        console.log(this.userCart);
      }
    }
  },
  mounted(){
    axios.get('https://fakestoreapi.com/products')
      .then((response) => {
        this.productList = response.data
      })
  }
}
</script>
