<template>
  <div>
  
  <div class="Bground">
    <div id="Maintitle"> <h1> Burgers!! </h1> </div>
    <div id="burgermenu"> 
      
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
              {{burgers.name}}
    </div>
    <div id="Sorders"> <h1> Sideorders </h1>
    <SideOrderitem v-for="sideOrderitem in sideorderitems"
          v-bind:sideOrderitem ="sideOrderitem"
          v-bind:key="sideOrderitem.name"/>
    
    </div>

    <div id="Dmenu"> <h1> Drinks</h1>
      <SideOrderitem v-for="sideOrderitem in drinks"
          v-bind:sideOrderitem ="sideOrderitem"
          v-bind:key="sideOrderitem.name"/>
          

    </div>

    <div id="Cinfo">
      <div v-bind:style="{'display':'inline'}">
      <h1 v-bind:style="{'grid-column': '2 '}"> Customer Information</h1>
     
      <p>
        <div>
        <label for="fullname">Full Name</label>
      </div>
      <input type="text" id="fullname" name="fn" required="required" placeholder="First and last name" v-model="fullname">
      </p>
      
      <p>
        <div>
        <label for="mail">E-mail</label>
      </div>
      <input type="text" id="mail" name="fn" required="required" placeholder="E-mail adress" v-model="mail"/>
      </p>

      <p>
        <div>
        <label for="adress">Street</label>
      </div>
      <input type="text" id="adress" name="fn" required="required" placeholder="Street name" v-model="adress"/>
      </p>

      <p>
        <div>
        <label for="hnumber">House Number</label>
      </div>
      <input type="number" id="hnumber" name="fn" required="required" placeholder="First name" v-model="hnumber">
      </p>
    
     

    <p>
      <div >
        <div>    
   <label for="payment">Payment Options</label>
        </div>
   <select  id="payment" name="pay" v-model="payment">
       <option>Credit card</option>
       <option>swish</option>
       <option>Pay pal</option>
       <option>Cash</option>
      
   </select>
  </div>
 


</p>

  <div>    
   <label for="gender">Gender</label>
        </div>
        <div>
  <input type="radio" id="gender" value="Male" v-model="gender"/>  <label for="male">Male</label> </div>
  <div>
  <input type="radio" id="gender" value = "female" v-model="gender"/> <label for="female">Female</label>
  </div>
  <div>
  <input type="radio" id="gender"  value = "nonbinary" v-model="gender"/>  <label for="nonbinary">Non Binary</label>   </div>
  <div>
  <input type="radio" id="gender"  value = "undisclosed" v-model="gender"/> <label for="undisclosed">Undisclosed</label>
</div>



 </div>

 <div id="map" v-on:click="setLocation" v-bind:style=" {background: 'url(' + require('../../public/img/polacks.jpg')+ ')'}" >
 
  <div  v-bind:style="{ left: locationx + 'px', top: locationy + 'px'}" v-bind:key="'map' ">
          T
        </div>
      
    </div>

</div>
    
   <div id="orderButton">
    <button v-on:click="addOrder" v-bind:style="{'width':'200px','height':'60px'}" > Place my order!</button>
   </div>
    
    
  </div>
</div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client' 
import SideOrderitem from '../components/SideOrders.vue'
import menu from '/Users/jason/Documents/github/1md031-lab-2022/src/assets/menu.json'


const socket = io(); 

function menuitems(n,kcal,alg){
  this.name = n;
  this.kalories = kcal;
  this.alergies;

}



export default { 
  
  name: 'HomeView',
  components: {
    Burger, SideOrderitem, 
  },
  data: function () {
    return {
      burgers : menu.burgers,
      infoitems: menu.inofoitems,
      deliveryTime: 0,
      orderedBurgers: {},
      fullname:"",
      hnumber:0,
      mail: "",
      gender:"",
      payment:"",
      adress:"",


      
      sideorderitems: menu.sideorderitems,    
      drinks: menu.drinks,
      locationx: 0,
      locationy: 0,
        
      //selecteditems: 
    }
      
  },

  created: function () {
      socket.on('deliveryTimes', time => this.deliveryTime = time);
      
    },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    setSelectedBurger: function(event){
       this.selectedburger = event.name;
    },
    addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
    console.log(event.name)
    

    },
    setLocation: function(event){
     var offset = { x : event.currentTarget.getBoundingClientRect().left,
     y: event.currentTarget.getBoundingClientRect().top,
     }

     this.locationy = event.clientY - 10 - offset.y;
     this.locationx = event.clientX - 10 - offset.x;
                   
                    
    },
    
    checkdeliveryTime: function(){
      socket.emit("deliveryTime")
    },
    addOrder: function (event) {
      var offset = { x : event.currentTarget.getBoundingClientRect().left,
     y: event.currentTarget.getBoundingClientRect().top,
     }
     
     console.log(this.mail,this.adress,this.gender);
      
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.locationx,
                                           y: this.locationy},
                                customerInfo:{ gender: this.gender, 
                                  name: this.fullname, 
                                  mail: this.mail,
                                  number: this.hnumber,
                                payment: this.payment ,
                              adress: this.adress},           
                                        
                                orderItems: this.orderedBurgers
                              }
                 );
                 console.log(this.orderedBurgers)

    }
  }
}

</script>

<style>
.Bground{
    display: grid;
    grid-template-columns: 1fr ;
    grid-auto-rows: minmax(100px, auto);
    border: blanchedalmond solid;
    padding: 100px;
    grid-row-gap: 30px;
    background-color: #350f0f;
  
    
  }
  #map {
    position: relative;
    grid-column: 2;
    overflow: scroll;
    width: 700px;
    height: 400px;
    
  }

  #map div {
    
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }


  #burgermenu{
    
    grid-column: 1;
    grid-row: span 5;
    *display: grid;
    padding-left: 350px;
    padding-right: 50px;
    
    
    grid-auto-rows: 50px;
    grid-auto-columns: 50px;
    
   
   
   
  }
  
  #Maintitle{
    grid-column: 1;
    font-weight: 200;
    font-style: normal;
    font-size: 30px;
    font-family: Arial, Helvetica, sans-serif;
    /*color: #402090;*/
    color: rgb(255, 85, 0);
    text-align: center;
   
    
    
  }
  #Sorders{
   
    grid-column: 1;
    /*background-color: burlywood;*/
    
    font-size: 24px;
    color: darkcyan;
    text-align: center;

    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;

  }
  #Dmenu{
    grid-column: 1;
    font-size: 24px;
    color: rgb(17, 167, 79);
    text-align: center;

    
    
    /*background-color: #bab4b4;*/

    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
  }
  #Cinfo{
    grid-column: 1;
    background-color: antiquewhite;
    padding: 40px;
    display: grid;
    grid-template-columns: 1fr 2fr ;
    padding: 50px;
    column-gap: 30 px;
  }
  #orderButton{
    grid-column: 1;
    padding-left: 500px;
  }

</style>