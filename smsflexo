
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SMS Flexo</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#0b0e14;color:#f1f5f9;font-family:Arial,sans-serif}
header{padding:22px;background:#121722;border-bottom:1px solid #303746}
header h1{margin:0;color:#64b5ff}
header p{color:#9ba7ba}
nav{display:flex;gap:10px;flex-wrap:wrap;margin-top:18px}
button{cursor:pointer;border:0;border-radius:9px;padding:12px 16px;background:#2585e8;color:white;font-weight:bold}
nav button{background:#252d3b}
main{max-width:900px;margin:auto;padding:20px}
.card{background:#151b27;border:1px solid #303746;border-radius:14px;padding:20px;margin:15px 0}
h2{margin-top:0}
input,select,textarea{display:block;width:100%;padding:12px;margin:10px 0 16px;border:1px solid #414c60;border-radius:8px;background:#0c111b;color:white}
label{color:#c4cede}
.stat{display:inline-block;background:#202b3b;padding:14px;border-radius:10px;margin:5px}
.hidden{display:none}
small{color:#aab5c6}
table{width:100%;border-collapse:collapse}
td,th{padding:10px;border-bottom:1px solid #343d4c;text-align:left}
footer{text-align:center;color:#8793a6;padding:30px}
</style>
</head>
<body>
<header>
<h1>SMS Flexo</h1>
<p>Your SMS request dashboard</p>
<nav>
<button onclick="show('home')">Dashboard</button>
<button onclick="show('request')">New Request</button>
<button onclick="show('history')">History</button>
<button onclick="show('admin')">Admin</button>
</nav>
</header>

<main>
<section id="home" class="page">
<div class="card">
<h2>Welcome to SMS Flexo</h2>
<p>Your account overview</p>
<div class="stat">Coins: <b id="coins">100</b></div>
<div class="stat">Requests: <b id="total">0</b></div>
<p><small>This is a demo website. No real phone numbers are supplied.</small></p>
</div>
<div class="card">
<h2>Account</h2>
<label>Username</label>
<input id="username" placeholder="Enter username">
<label>Email</label>
<input id="email" type="email" placeholder="Enter email">
<button onclick="saveProfile()">Save Profile</button>
<p id="profileMsg"></p>
</div>
</section>

<section id="request" class="page hidden">
<div class="card">
<h2>Request a Number</h2>
<label>Country</label>
<select id="country">
<option>United Kingdom</option><option>United States</option>
<option>Canada</option><option>Pakistan</option>
</select>
<label>Service</label>
<select id="service">
<option>WhatsApp</option><option>Facebook</option>
<option>Telegram</option><option>Other</option>
</select>
<label>Number type</label>
<select id="type">
<option>Standard</option><option>F1</option><option>F2</option>
</select>
<label>Quantity</label>
<input id="quantity" type="number" min="1" max="10" value="1">
<label>Notes (optional)</label>
<textarea id="notes" placeholder="Additional details"></textarea>
<button onclick="submitRequest()">Submit Request</button>
<p id="requestMsg"></p>
<small>Requests are demo records. This site is not connected to a real SMS provider.</small>
</div>
</section>

<section id="history" class="page hidden">
<div class="card">
<h2>Request History</h2>
<div id="historyList">No requests yet.</div>
</div>
</section>

<section id="admin" class="page hidden">
<div class="card">
<h2>Admin Dashboard</h2>
<p><small>Demo only: this email check is not secure and does not provide real admin protection.</small></p>
<label>Admin email</label>
<input id="adminEmail" type="email" placeholder="Enter admin email">
<button onclick="openAdmin()">Open Admin</button>
<div id="adminArea" class="hidden">
<h3>Coin Management</h3>
<label>User email</label>
<input id="targetEmail" type="email" placeholder="User email">
<label>Coin amount (use a negative number to deduct)</label>
<input id="amount" type="number" value="10">
<label>Reason</label>
<input id="reason" placeholder="Reason for adjustment">
<button onclick="adjustCoins()">Adjust Demo Coins</button>
<h3>Requests</h3>
<div id="adminRequests"></div>
<div id="adminMsg"></div>
</div>
</div>
</section>
</main>

<footer>SMS Flexo — Demo Version</footer>

<script>
const ADMIN_EMAIL="arfanasir2525@gmail.com";
const getData=(key,def)=>{try{return JSON.parse(localStorage.getItem(key))??def}catch{return def}};
const setData=(key,value)=>localStorage.setItem(key,JSON.stringify(value));
let profile=getData("sf_profile",{username:"",email:""});
let requests=getData("sf_requests",[]);
let coins=getData("sf_coins",100);
let adjustments=getData("sf_adjustments",[]);
function show(id){
document.querySelectorAll(".page").forEach(p=>p.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
if(id==="history")renderHistory();
}
function saveProfile(){
profile.username=document.getElementById("username").value.trim();
profile.email=document.getElementById("email").value.trim().toLowerCase();
if(!profile.username||!profile.email){profileMsg.textContent="Enter username and email.";return}
setData("sf_profile",profile);
profileMsg.textContent="Demo profile saved on this device.";
}
function submitRequest(){
const q=Number(document.getElementById("quantity").value);
if(!Number.isInteger(q)||q<1||q>10){requestMsg.textContent="Quantity must be 1–10.";return}
if(!profile.email){requestMsg.textContent="First save your profile on Dashboard.";return}
const r={id:Date.now(),country:country.value,service:service.value,type:type.value,quantity:q,notes:notes.value,status:"Pending",created:Date.now(),expires:Date.now()+20*60*1000,email:profile.email,username:profile.username};
requests.unshift(r);setData("sf_requests",requests);
total.textContent=requests.length;
requestMsg.textContent="Demo request submitted. It expires in 20 minutes.";
renderHistory();
}
function renderHistory(){
requests=getData("sf_requests",[]);
historyList.innerHTML=requests.filter(r=>r.email===profile.email).map(r=>{
const left=Math.max(0,Math.ceil((r.expires-Date.now())/1000));
const timer=left?Math.floor(left/60)+":"+String(left%60).padStart(2,"0"):"Expired";
return `<div class="card"><b>${esc(r.service)}</b> — ${esc(r.country)}<p>Type: ${esc(r.type)} | Quantity: ${r.quantity}</p><p>Status: ${esc(r.status)}</p><p>Time left: ${timer}</p></div>`;
}).join("")||"No requests yet.";
}
function openAdmin(){
if(document.getElementById("adminEmail").value.trim().toLowerCase()!==ADMIN_EMAIL){alert("Admin email does not match.");return}
document.getElementById("adminArea").classList.remove("hidden");
renderAdmin();
}
function renderAdmin(){
requests=getData("sf_requests",[]);
adminRequests.innerHTML=requests.map(r=>{
if(Date.now()>=r.expires&&r.status==="Pending")r.status="Expired";
return `<div class="card"><b>${esc(r.username||"User")}</b> (${esc(r.email)})<p>${esc(r.country)} · ${esc(r.service)} · ${esc(r.type)} · Qty ${r.quantity}</p><p>Status: ${esc(r.status)}</p>${r.status==="Pending"?`<button onclick="setStatus(${r.id},'Approved')">Approve</button> <button onclick="setStatus(${r.id},'Rejected')">Reject</button>`:""}</div>`;
}).join("")||"No requests.";
setData("sf_requests",requests);
}
function setStatus(id,status){
requests=getData("sf_requests",[]);
const r=requests.find(x=>x.id===id);
if(!r)return;
if(Date.now()>=r.expires){r.status="Expired"}else{r.status=status}
setData("sf_requests",requests);renderAdmin();
}
function adjustCoins(){
const email=targetEmail.value.trim().toLowerCase();
const amountNum=Number(amount.value);
if(!email||!Number.isFinite(amountNum)||amountNum===0){adminMsg.textContent="Enter a user email and non-zero amount.";return}
if(email===profile.email){
coins=Math.max(0,coins+amountNum);setData("sf_coins",coins);
document.getElementById("coins").textContent=coins;
}
adjustments.unshift({email,amount:amountNum,reason:reason.value,time:new Date().toLocaleString()});
setData("sf_adjustments",adjustments);
adminMsg.textContent="Demo adjustment recorded. Only this device's demo balance can be changed.";
}
function esc(s){return String(s??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
document.getElementById("username").value=profile.username;
document.getElementById("email").value=profile.email;
document.getElementById("coins").textContent=coins;
document.getElementById("total").textContent=requests.length;
setInterval(()=>{if(!document.getElementById("history").classList.contains("hidden"))renderHistory()},1000);
</script>
</body>
</html>
