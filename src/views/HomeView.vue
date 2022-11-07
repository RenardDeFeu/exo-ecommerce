<template>
  <div class="home">
    <img class="inline" alt="Vue logo" src="../assets/logo.png">
    <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
    Number of articles in your cart : {{ userCart.length }}
    <div class="grid grid-cols-4 gap-4 mx-10">
      <div :class="[cartButton && divID == index? 'inline border border-blue-600 rounded-lg shadow-lg mb-auto' : 'inline border p-2 rounded-lg shadow-md mb-36']" v-bind:key="product.id" 
            v-for="(product, index) in productList" @mouseover="addCartButton(index)" @mouseleave="cartButton = false"> <!--v-on:click="addCartButton(index)" mb-36-->
        {{ product.title }} 
        <br>
        <p> <img class="max-h-32 max-w-[25%] inline" v-bind:src="product.image"> </p>
        $ {{ product.price }}
        <div v-if="cartButton && divID == index">
          <ProductDetails/> <button class="rounded-lg bg-transparent text-black ml-2 p-2 hover:bg-red-100" v-on:click="null">See the details</button>
          <br>
          <button class="rounded-lg bg-blue-600 text-white ml-2 p-2 hover:bg-red-500" v-on:click="addToCart(product)">Add To Cart</button>
          <button class="rounded-lg bg-blue-600 text-white ml-2 p-2 hover:bg-red-500" v-on:click="removeToCart(product)">Remove from Cart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
//import HelloWorld from '@/components/HelloWorld.vue'
import ProductDetails from '@/components/ProductDetails.vue'
import axios from 'axios'

export default {
  name: 'HomeView',
  components: {
    //HelloWorld
    ProductDetails
  },
  data()
  {
    return {
      productList: [],
      cartButton: false,
      divID: null,

      userCart: [],
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
