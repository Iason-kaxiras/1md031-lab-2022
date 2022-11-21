<template>
    <div class="orderMenu" style="display: inline-block;" > 
        <div class="menuItem">
         <h2> {{sideOrderitem.name}} </h2>
         {{ sideOrderitem.kcal }}kCal
         <h2> {{sideOrderitem.cost}} kr</h2> 
         
        </div> 
        <div>
         amount
         <button v-on:click="addItem">+</button>
         {{this.amountOrdered}}
         <button v-on:click="deleteItem">-</button>
        </div> 
               </div>
        

   
  </template>

  <script>
 export default {
    name: 'SideOrders',
    props: {
      sideOrderitem: Object
    },
    data: function(){
       return{
       amountOrdered:0,
       }
    },
    methods:{
        selectItem: function(){
                this.$emit("selected",this.sideOrderitem.name)
        },
        addItem: function(){
        this.amountOrdered = this.amountOrdered + 1;
        console.log(this.amountOrdered)
      },
      deleteItem: function(){
        this.amountOrdered = this.amountOrdered - 1;
        console.log(this.amountOrdered)
      }


    }
}

  </script>

  <style>
    .orderMenu{
        background-color: rgb(118, 109, 109);
        font-size: 14px;
        margin: 10px;
        height: 180px;
        width: 180px;
        
        color: black;
        vertical-align: top;
        border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    border-bottom-left-radius: 10px;
    border-bottom-right-radius: 10px;

    }
    #sbutton{
      color: red;

    }

  </style>