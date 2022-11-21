<template>
    <div class="burger" v-bind:style="{'background-image': 'url('+burger.image+')','opacity': 1}" > 
      <div id="bname"><h1> {{ burger.name }}</h1>
           </div>
       <h3>{{ burger.kCal }}kCal </h3>
        <h2> {{burger.cost}} kr</h2>   
            <div id="ingrid" > <h3> ingridience </h3>
             <ul v-for="btn in burger.ingridience">
             <li> {{btn}}</li>
             </ul>
                
            <div id ="aha"></div>
         </div> 
         amount
         <button v-on:click="addBurger">+</button>
         {{this.amountOrdered}}
         <button v-on:click="deleteBurger">-</button>
        </div> 

   
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data:function(){
      return{
        amountOrdered: 0,
      }

    },
    methods:{
      
      addBurger: function(){
        this.amountOrdered = this.amountOrdered + 1;
        this.$emit('orderedBurger', {name: this.burger.name, 
                                amount:this.amountOrdered});
        
      },
      deleteBurger: function(){
        this.amountOrdered = this.amountOrdered - 1;
        this.$emit("orderedBurger", {name: this.burger.name, 
                                amount:this.amountOrdered});
        
      }
      
     

     }
    }
  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .burger{
    
    display: inline-block;
    height: 500px;
    width: 300px;
    background-color: beige;
    color: rgb(60, 109, 195);
    
    font-size: 12pt;
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    text-align: center;
    vertical-align: top;
    margin: 10px;
    border-top-left-radius: 40px;
    border-top-right-radius: 40px;
    border-bottom-left-radius: 40px;
    border-bottom-right-radius: 40px;
}

#bname{
  
  font-style:oblique;
  text-shadow: 10px;
  color: brown;
  
}
#aha{
  background-color: orangered;
}

#ingrid{
  background-color: beige;
  width: 160px;
  margin-left: 90px;
  text-align: left;
  padding-left: 10px;
  padding-top: 1px;
  padding-bottom: 2px;

  border-top-left-radius: 40px;
    border-top-right-radius: 40px;
    border-bottom-left-radius: 40px;
    border-bottom-right-radius: 40px;
  
  
}

  </style>
  