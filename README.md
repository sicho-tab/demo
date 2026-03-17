<!DOCTYPE html>
<html>
<head>
<title>Nerdy Retro Store</title>
<style>
body{
 background:#0a0a0a;
 color:#00ffcc;
 font-family:"Comic Sans MS", cursive;
 text-align:center;
}
.container{border:2px solid #00ffcc;margin:15px;padding:15px;border-radius:10px}
.nav a{color:#ffcc00;margin:10px;cursor:pointer;font-weight:bold}
.product{
 border:2px solid #ff00ff;
 margin:15px;
 padding:10px;
 border-radius:10px;
 background:#111;
}
button{
 background:#ff0066;
 color:white;
 border:none;
 padding:6px 12px;
 margin:5px;
 cursor:pointer;
 border-radius:5px;
}
button:hover{background:#ff3399}
.hidden{display:none}
.footer{margin-top:30px;font-size:12px}
.header{
 font-size:22px;
 color:#ffcc00;
 text-shadow:2px 2px #ff00ff;
}
.back{
 position:absolute;
 left:20px;
 top:20px;
 cursor:pointer;
 color:#00ffcc;
}
</style>
</head>
<body>

<div class="header">🧠 Nerdy Comic Store</div>
<div class="nav">
<a onclick="showPage('home')">Home</a>
<a onclick="showPage('products')">Shop</a>
<a onclick="showPage('cart')">Cart (<span id='cartCount'>0</span>)</a>
<a onclick="showPage('orders')">Orders</a>
</div>

<div id="home" class="container">
<h2>Welcome Geek 👾</h2>
<p>Comics. Collectibles. Pure Nerd Energy.</p>
</div>

<div id="products" class="container hidden"></div>

<div id="cart" class="container hidden">
<div class="back" onclick="showPage('products')">⬅ Back</div>
<h2>🛒 Your Cart</h2>
<div id="cartItems"></div>
<p>Total ₹<span id="total">0</span></p>
<button onclick="showPage('shipping')">Proceed</button>
</div>

<div id="shipping" class="container hidden">
<div class="back" onclick="showPage('cart')">⬅ Back</div>
<h2>🚚 Shipping</h2>
<input id="name" placeholder="Name"><br><br>
<input id="address" placeholder="Address"><br><br>
<input id="phone" placeholder="Phone"><br><br>
<button onclick="saveShipping()">Continue</button>
</div>

<div id="payment" class="container hidden">
<div class="back" onclick="showPage('shipping')">⬅ Back</div>
<h2>💳 Payment</h2>
<button onclick="placeOrder('COD')">Cash on Delivery</button>
<button onclick="placeOrder('UPI')">UPI</button>
</div>

<div id="orders" class="container hidden">
<h2>📦 Orders</h2>
<div id="orderList"></div>
</div>

<div id="confirmation" class="container hidden">
<h2>🎉 Order Confirmed</h2>
<p id="orderDetails"></p>
<button onclick="sendWhatsApp()">Send on WhatsApp</button>
<button onclick="showPage('home')">Home</button>
</div>

<div class="footer">© Nerdy Store 1999</div>

<script>
let cart = JSON.parse(localStorage.getItem('cart')) || [];
let orders = JSON.parse(localStorage.getItem('orders')) || [];
let shippingData = {};

const products = [
 {name:'Spider Comic',price:299},
 {name:'Anime Figure',price:799},
 {name:'Marvel Poster',price:199}
];

function renderProducts(){
 let html='';
 products.forEach(p=>{
  html+=`<div class='product'>
  <h3>${p.name}</h3>
  <p>₹${p.price}</p>
  <button onclick="addToCart('${p.name}',${p.price})">Add to Cart</button>
  <button onclick="buyNow('${p.name}',${p.price})">Buy Now</button>
  </div>`;
 });
 document.getElementById('products').innerHTML=html;
}

function showPage(p){
 document.querySelectorAll('.container').forEach(d=>d.classList.add('hidden'));
 document.getElementById(p).classList.remove('hidden');
}

function addToCart(n,p){
 cart.push({n,p});
 saveCart();
}

function buyNow(n,p){
 cart=[{n,p}];
 saveCart();
 showPage('shipping');
}

function saveCart(){
 localStorage.setItem('cart',JSON.stringify(cart));
 updateCart();
}

function updateCart(){
 document.getElementById('cartCount').innerText=cart.length;
 let total=0,html='';
 cart.forEach(i=>{total+=i.p;html+=`<p>${i.n} - ₹${i.p}</p>`});
 document.getElementById('cartItems').innerHTML=html;
 document.getElementById('total').innerText=total;
}

function saveShipping(){
 shippingData={
  name:document.getElementById('name').value,
  address:document.getElementById('address').value,
  phone:document.getElementById('phone').value
 };
 showPage('payment');
}

function placeOrder(method){
 const order={id:Date.now(),cart,shippingData,method};
 orders.push(order);
 localStorage.setItem('orders',JSON.stringify(orders));
 cart=[]; saveCart();
 document.getElementById('orderDetails').innerText=`Order #${order.id} via ${method}`;
 showPage('confirmation');
 renderOrders();
}

function renderOrders(){
 let html='';
 orders.forEach(o=>{
  html+=`<p>Order #${o.id} - ${o.method}</p>`;
 });
 document.getElementById('orderList').innerHTML=html;
}

function sendWhatsApp(){
 const msg="I just placed an order from Nerdy Store!";
 window.open(`https://wa.me/?text=${encodeURIComponent(msg)}`);
}

renderProducts();
updateCart();
renderOrders();
</script>

</body>
</html>
