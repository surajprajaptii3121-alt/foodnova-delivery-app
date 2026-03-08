
<!DOCTYPE html>
<html>
<head>
<title>FoodNova</title>

<style>

body{
font-family:Arial;
margin:0;
background:#f2f2f2;
}

header{
background:#ff6600;
color:white;
padding:15px;
font-size:24px;
}

.container{
padding:20px;
}

.restaurant{
background:white;
margin-bottom:15px;
padding:15px;
border-radius:10px;
box-shadow:0 0 5px #ccc;
}

.menu-item{
display:flex;
justify-content:space-between;
margin:5px 0;
}

button{
background:#ff6600;
color:white;
border:none;
padding:6px 10px;
border-radius:5px;
cursor:pointer;
}

input{
padding:5px;
margin:5px;
}

.cart{
position:fixed;
right:20px;
top:80px;
width:260px;
background:white;
padding:10px;
border-radius:10px;
border:1px solid #ccc;
}

.login{
background:white;
padding:15px;
margin:20px;
border-radius:10px;
}

.admin{
background:white;
padding:15px;
margin:20px;
border-radius:10px;
}

</style>

</head>

<body>

<header>FoodNova Delivery</header>

<div id="loginBox" class="login">

<h3>Login / Signup</h3>

<input id="username" placeholder="Username">
<br>
<button onclick="login()">Enter</button>

</div>

<div id="app" style="display:none">

<div class="container" id="restaurantList"></div>

<div class="cart">

<h3>Cart</h3>

<ul id="cartItems"></ul>

<h4 id="total">Total ₹0</h4>

<button onclick="placeOrder()">Order</button>

<h4>Orders</h4>

<ul id="orders"></ul>

<h4>Location</h4>

<button onclick="getLocation()">Get Location</button>

<p id="location"></p>

</div>

<div class="admin">

<h3>Admin Panel</h3>

<input id="restName" placeholder="Restaurant name">

<input id="foodName" placeholder="Food name">

<input id="foodPrice" placeholder="Price">

<button onclick="addRestaurant()">Add Restaurant</button>

</div>

</div>

<script>

let restaurants=[

{
name:"Pizza Corner",
menu:[
{name:"Cheese Pizza",price:200},
{name:"Veg Pizza",price:180}
]
},

{
name:"Burger Town",
menu:[
{name:"Veg Burger",price:120},
{name:"Cheese Burger",price:150}
]
}

]

let cart=[]
let total=0
let user=""

function login(){

user=document.getElementById("username").value

if(user==""){
alert("Enter username")
return
}

document.getElementById("loginBox").style.display="none"
document.getElementById("app").style.display="block"

loadRestaurants()
loadOrders()

}

function loadRestaurants(){

let html=""

restaurants.forEach((r)=>{

html+=`<div class="restaurant">
<h3>${r.name}</h3>`

r.menu.forEach((m)=>{

html+=`

<div class="menu-item">

<span>${m.name} ₹${m.price}</span>

<button onclick="addCart('${m.name}',${m.price})">Add</button>

</div>

`

})

html+="</div>"

})

document.getElementById("restaurantList").innerHTML=html

}

function addCart(name,price){

cart.push({name,price})
total+=price

renderCart()

}

function renderCart(){

let html=""

cart.forEach((c)=>{

html+=`<li>${c.name} ₹${c.price}</li>`

})

document.getElementById("cartItems").innerHTML=html

document.getElementById("total").innerText="Total ₹"+total

}

function placeOrder(){

if(cart.length==0){

alert("Cart empty")
return

}

let orders=JSON.parse(localStorage.getItem("orders")||"[]")

orders.push({user,total})

localStorage.setItem("orders",JSON.stringify(orders))

cart=[]
total=0

renderCart()
loadOrders()

alert("Order placed!")

}

function loadOrders(){

let orders=JSON.parse(localStorage.getItem("orders")||"[]")

let html=""

orders.forEach((o)=>{

html+=`<li>${o.user} - ₹${o.total}</li>`

})

document.getElementById("orders").innerHTML=html

}

function addRestaurant(){

let rname=document.getElementById("restName").value
let fname=document.getElementById("foodName").value
let price=parseInt(document.getElementById("foodPrice").value)

restaurants.push({

name:rname,
menu:[{name:fname,price:price}]

})

loadRestaurants()

}

function getLocation(){

if(navigator.geolocation){

navigator.geolocation.getCurrentPosition((pos)=>{

document.getElementById("location").innerText=

pos.coords.latitude+" , "+pos.coords.longitude

})

}

}

</script>

</body>
</html>


