# e-wash-business-manager
AI-assisted business dashboard for E-Wash Laundry
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>E-Wash Laundry | Business Manager</title>
<style>
:root{--blue:#123c78;--blue2:#1769aa;--yellow:#ffd43b;--bg:#f3f6fb;--ink:#18243a;--muted:#69758a;--white:#fff;--border:#e1e7f0;--green:#16845b;--red:#c33}
*{box-sizing:border-box}
body{margin:0;font-family:Arial,Helvetica,sans-serif;background:var(--bg);color:var(--ink)}
button,input,select,textarea{font:inherit}
button{cursor:pointer}
.top{background:linear-gradient(120deg,var(--blue),var(--blue2));color:white;padding:20px 16px}
.top-inner{max-width:1200px;margin:auto;display:flex;align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap}
.brand{display:flex;gap:12px;align-items:center}
.logo{width:48px;height:48px;border-radius:14px;background:var(--yellow);color:var(--blue);display:grid;place-items:center;font-size:25px;font-weight:bold}
.brand h1{font-size:20px;margin:0 0 5px}
.brand p{font-size:12px;margin:0;color:#e5efff}
.top-actions{display:flex;gap:8px;flex-wrap:wrap}
.wrap{max-width:1200px;margin:0 auto;padding:16px}
.nav{display:flex;gap:8px;overflow-x:auto;padding:10px 0 16px}
.nav button{white-space:nowrap;border:1px solid var(--border);background:white;border-radius:10px;padding:10px 13px;color:var(--blue);font-weight:700}
.nav button.active{background:var(--blue);color:white}
h2{font-size:22px;margin:8px 0 5px}
.sub{color:var(--muted);font-size:13px;margin:0 0 16px}
.grid{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:12px}
.two{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.card{background:white;border:1px solid var(--border);border-radius:15px;padding:16px;margin-bottom:14px;box-shadow:0 3px 14px #193a6810;min-width:0}
.kpi{font-size:26px;font-weight:800;margin:9px 0 5px;overflow-wrap:anywhere}
.kpi-label{font-size:12px;color:var(--muted)}
.kpi-icon{font-size:22px}
h3{margin:0 0 12px;font-size:16px}
label{display:block;font-size:12px;font-weight:700;color:#526078;margin:10px 0 5px}
input,select,textarea{width:100%;border:1px solid #d5deeb;border-radius:9px;padding:10px;background:white;color:var(--ink);min-width:0}
textarea{min-height:90px;resize:vertical}
.row{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}
.btn{border:0;background:var(--blue);color:white;border-radius:9px;padding:10px 13px;font-weight:700}
.btn.yellow{background:var(--yellow);color:#152f56}
.btn.light{background:#edf3fc;color:var(--blue)}
.btn.red{background:#ffeded;color:#a22}
.btn.green{background:#e5f7ef;color:#08764d}
.btn.small{font-size:12px;padding:7px 9px}
.actions{display:flex;gap:6px;flex-wrap:wrap;margin-top:10px}
.pill{display:inline-block;padding:5px 8px;border-radius:99px;font-size:11px;font-weight:700;background:#edf3fc;color:var(--blue)}
.pill.green{background:#e5f7ef;color:#08764d}
.pill.yellow{background:#fff5c8;color:#795700}
.pill.red{background:#ffeded;color:#a22}
.notice{background:#fff7d6;border:1px solid #f3df83;padding:12px;border-radius:11px;font-size:13px;line-height:1.5;margin-bottom:14px}
table{width:100%;border-collapse:collapse;font-size:13px}
th,td{text-align:left;padding:10px 8px;border-bottom:1px solid var(--border);vertical-align:top}
th{color:var(--muted);font-size:11px;text-transform:uppercase}
.table-wrap{overflow-x:auto}
.empty{padding:20px;text-align:center;color:var(--muted);font-size:13px}
.item{padding:12px 0;border-bottom:1px solid var(--border)}
.item:last-child{border-bottom:0}
.bar-row{display:grid;grid-template-columns:110px 1fr 35px;gap:8px;align-items:center;margin:12px 0;font-size:12px}
.bar{height:12px;background:#e8edf5;border-radius:99px;overflow:hidden}
.bar span{display:block;height:100%;background:var(--blue2);border-radius:99px}
.footer{text-align:center;color:var(--muted);font-size:11px;padding:20px}
.hidden{display:none!important}
small{color:var(--muted)}
hr{border:0;border-top:1px solid var(--border);margin:14px 0}
@media(max-width:760px){.grid{grid-template-columns:repeat(2,minmax(0,1fr))}.two{grid-template-columns:1fr}.wrap{padding:12px}.card{padding:13px}.kpi{font-size:23px}.brand h1{font-size:18px}}
@media(max-width:390px){.grid{grid-template-columns:1fr 1fr}.row{grid-template-columns:1fr}.top{padding:15px 12px}}
@media print{.top,.nav,.no-print,.footer{display:none!important}body{background:white}.wrap{max-width:none;padding:0}.card{box-shadow:none;border:0}.print-only{display:block!important}}
</style>
</head>
<body>
<header class="top">
 <div class="top-inner">
  <div class="brand"><div class="logo">EW</div><div><h1>E-Wash Laundry</h1><p>Your Clothes, Our Care · Business Manager</p></div></div>
  <div class="top-actions">
   <button class="btn yellow" onclick="exportBackup()">⬇ Backup</button>
   <button class="btn light" onclick="document.getElementById('restoreFile').click()">⬆ Restore</button>
   <input id="restoreFile" type="file" accept=".json,application/json" class="hidden" onchange="restoreBackup(event)">
  </div>
 </div>
</header>
<main class="wrap">
 <div class="notice"><b>Setup status:</b> This is a working browser-based manager. Orders, customers and records are stored on this device. Google Business Profile and WhatsApp are <b>not connected</b>. Do not enter passwords, OTPs, card details or sensitive customer information here.</div>
 <nav class="nav no-print" id="nav">
  <button class="active" data-page="dashboard">🏠 Dashboard</button>
  <button data-page="orders">🧺 Orders</button>
  <button data-page="customers">👥 Customers</button>
  <button data-page="pricing">💰 Pricing</button>
  <button data-page="invoice">🧾 Invoice</button>
  <button data-page="marketing">📣 Content Studio</button>
  <button data-page="reviews">⭐ Reviews</button>
  <button data-page="inbox">💬 Enquiries</button>
  <button data-page="analytics">📊 Analytics</button>
  <button data-page="settings">⚙️ Settings</button>
 </nav>
 <section id="app"></section>
 <div class="footer">E-Wash Laundry · Local browser storage · Export a backup regularly</div>
</main>
<script>
"use strict";
const KEY="ewash_manager_v1";
const defaultState={
 settings:{business:"E-Wash Laundry",phone:"+91 9867542518",address:"Saki Vihar Road, Powai, Mumbai – 400072",website:"https://e-wash-laundry.grexa.site",reviewLink:"https://g.page/r/CTWlsx8JNOpyEAE/review"},
 prices:[
 {name:"Wash & Fold",rate:99,unit:"kg"},
 {name:"Wash + Dry + Iron",rate:160,unit:"kg"},
 {name:"Dry Wash + Steam Iron",rate:220,unit:"kg"},
 {name:"Premium Wash + Steam Iron",rate:250,unit:"kg"},
 {name:"Blazer Steam Iron",rate:180,unit:"item"},
 {name:"Blazer Wash + Iron",rate:250,unit:"item"},
 {name:"Blazer Dry Cleaning / Special Care",rate:300,unit:"item"}
 ],
 orders:[],customers:[],reviews:[],enquiries:[],posts:[],metrics:{views:0,searches:0,interactions:0,calls:0,directions:0,website:0}
};
let state=loadState(),page="dashboard";
function loadState(){try{return Object.assign(structuredClone(defaultState),JSON.parse(localStorage.getItem(KEY)||"{}"))}catch(e){return structuredClone(defaultState)}}
function save(){localStorage.setItem(KEY,JSON.stringify(state))}
function esc(v){return String(v??"").replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
function money(n){return "₹"+Number(n||0).toLocaleString("en-IN",{maximumFractionDigits:2})}
function today(){return new Date().toISOString().slice(0,10)}
function id(prefix){return prefix+"-"+Date.now().toString().slice(-7)}
function val(id){return document.getElementById(id)?.value?.trim()||""}
function num(id){return Number(val(id))||0}
function options(items,current){return items.map(x=>`<option ${x===current?"selected":""} value="${esc(x)}">${esc(x)}</option>`).join("")}
function pill(text,kind=""){return `<span class="pill ${kind}">${esc(text)}</span>`}
function card(title,content){return `<div class="card"><h3>${title}</h3>${content}</div>`}
function kpi(label,value,icon){return `<div class="card"><div class="kpi-icon">${icon}</div><div class="kpi">${value}</div><div class="kpi-label">${label}</div></div>`}
function setPage(p){page=p;document.querySelectorAll("#nav button").forEach(b=>b.classList.toggle("active",b.dataset.page===p));render()}
document.getElementById("nav").addEventListener("click",e=>{const b=e.target.closest("button[data-page]");if(b)setPage(b.dataset.page)});
function render(){
 const root=document.getElementById("app");
 const pages={dashboard:dashboardPage,orders:ordersPage,customers:customersPage,pricing:pricingPage,invoice:invoicePage,marketing:marketingPage,reviews:reviewsPage,inbox:inboxPage,analytics:analyticsPage,settings:settingsPage};
 root.innerHTML=(pages[page]||dashboardPage)();
}
function dashboardPage(){
 const revenue=state.orders.filter(o=>o.payment==="Paid").reduce((s,o)=>s+Number(o.total),0);
 const pending=state.orders.filter(o=>!["Delivered","Cancelled"].includes(o.status)).length;
 const unpaid=state.orders.filter(o=>o.payment!=="Paid").reduce((s,o)=>s+Number(o.total),0);
 const latest=[...state.orders].reverse().slice(0,5);
 return `<h2>Business overview</h2><p class="sub">Your laundry operations at a glance.</p>
 <div class="grid">${kpi("Total orders",state.orders.length,"🧺")}${kpi("Paid revenue",money(revenue),"💰")}${kpi("Active orders",pending,"🚚")}${kpi("Unpaid balance",money(unpaid),"🧾")}</div>
 <div class="two">${card("Recent orders",latest.length?`<div class="table-wrap"><table><tr><th>Order</th><th>Customer</th><th>Status</th><th>Total</th></tr>${latest.map(o=>`<tr><td>${esc(o.id)}</td><td>${esc(o.customer)}</td><td>${pill(o.status,o.status==="Delivered"?"green":"yellow")}</td><td>${money(o.total)}</td></tr>`).join("")}</table></div>`:`<div class="empty">No orders yet. Add your first order in Orders.</div>`)}
 ${card("Quick actions",`<div class="actions"><button class="btn" onclick="setPage('orders')">＋ New order</button><button class="btn light" onclick="setPage('customers')">＋ Customer</button><button class="btn light" onclick="setPage('invoice')">Create invoice</button><button class="btn light" onclick="setPage('marketing')">Create post draft</button></div><hr><p>📍 ${esc(state.settings.address)}</p><p>☎ ${esc(state.settings.phone)}</p><p><span class="pill yellow">Google profile: not connected</span> <span class="pill yellow">WhatsApp API: not connected</span></p>`)}
 </div>
 <div class="two">${card("Order status",statusBars())}${card("Business links",`<p><a href="${esc(state.settings.website)}" target="_blank" rel="noopener">Open website ↗</a></p><p><a href="${esc(state.settings.reviewLink)}" target="_blank" rel="noopener">Open Google review link ↗</a></p><button class="btn light" onclick="setPage('settings')">Edit business details</button>`)}</div>`;
}
function statusBars(){
 const statuses=["New","Pickup Scheduled","Washing","Ready","Out for Delivery","Delivered","Cancelled"];
 const max=Math.max(1,...statuses.map(s=>state.orders.filter(o=>o.status===s).length));
 return statuses.map(s=>{let n=state.orders.filter(o=>o.status===s).length;return `<div class="bar-row"><span>${s}</span><div class="bar"><span style="width:${n/max*100}%"></span></div><b>${n}</b></div>`}).join("");
}
function ordersPage(){
 return `<h2>Orders</h2><p class="sub">Create, update and track laundry orders.</p>
 ${card("Add new order",`<div class="row"><div><label>Customer name *</label><input id="ocustomer" placeholder="Customer or hotel name"></div><div><label>Phone (optional)</label><input id="ophone" placeholder="Phone number"></div><div><label>Service *</label><select id="oservice">${state.prices.map(p=>`<option value="${esc(p.name)}">${esc(p.name)} — ${money(p.rate)}/${esc(p.unit)}</option>`).join("")}</select></div><div><label>Quantity (kg or items) *</label><input id="oqty" type="number" min="0.1" step="0.1" value="1"></div><div><label>Pickup / order date</label><input id="odate" type="date" value="${today()}"></div><div><label>Delivery date</label><input id="odelivery" type="date"></div><div><label>Status</label><select id="ostatus">${options(["New","Pickup Scheduled","Washing","Ready","Out for Delivery","Delivered","Cancelled"],"New")}</select></div><div><label>Payment status</label><select id="opayment">${options(["Unpaid","Part Paid","Paid"],"Unpaid")}</select></div></div><label>Notes (optional)</label><textarea id="onotes" placeholder="Stain, fabric care, delivery instructions..."></textarea><div class="actions"><button class="btn" onclick="addOrder()">＋ Save order</button></div>`)}
 ${card("Order register",state.orders.length?`<div class="table-wrap"><table><tr><th>Order / date</th><th>Customer / service</th><th>Qty</th><th>Total</th><th>Status / payment</th><th>Actions</th></tr>${[...state.orders].reverse().map(o=>`<tr><td><b>${esc(o.id)}</b><br><small>${esc(o.date)}</small></td><td>${esc(o.customer)}<br><small>${esc(o.service)}</small></td><td>${o.qty}</td><td>${money(o.total)}</td><td><select onchange="updateOrder('${o.id}','status',this.value)">${options(["New","Pickup Scheduled","Washing","Ready","Out for Delivery","Delivered","Cancelled"],o.status)}</select><br><select style="margin-top:5px" onchange="updateOrder('${o.id}','payment',this.value)">${options(["Unpaid","Part Paid","Paid"],o.payment)}</select></td><td><div class="actions"><button class="btn small light" onclick="useInvoice('${o.id}')">Invoice</button><button class="btn small red" onclick="deleteOrder('${o.id}')">Delete</button></div></td></tr>`).join("")}</table></div>`:`<div class="empty">No orders saved yet.</div>`)}
 `;
}
function addOrder(){
 const customer=val("ocustomer"),service=val("oservice"),qty=num("oqty");
 if(!customer||!service||qty<=0){alert("Enter customer name, service and a quantity above zero.");return}
 const price=state.prices.find(p=>p.name===service);if(!price){alert("Choose a valid service.");return}
 const phone=val("ophone");
 let c=state.customers.find(x=>x.name.toLowerCase()===customer.toLowerCase());
 if(!c){c={id:id("CUS"),name:customer,phone,notes:""};state.customers.push(c)}else if(phone&&!c.phone)c.phone=phone;
 state.orders.push({id:id("EW"),customer,phone,service,qty,rate:Number(price.rate),total:Math.round(qty*Number(price.rate)*100)/100,date:val("odate")||today(),delivery:val("odelivery"),status:val("ostatus"),payment:val("opayment"),notes:val("onotes")});
 save();render();alert("Order saved.");
}
function updateOrder(orderId,field,value){const o=state.orders.find(x=>x.id===orderId);if(o){o[field]=value;save();render()}}
function deleteOrder(orderId){if(confirm("Delete this order?")){state.orders=state.orders.filter(o=>o.id!==orderId);save();render()}}
function customersPage(){
 return `<h2>Customers / CRM</h2><p class="sub">Keep customer contact details and order history.</p>
 ${card("Add customer",`<div class="row"><div><label>Name *</label><input id="cname" placeholder="Customer name"></div><div><label>Phone</label><input id="cphone" placeholder="Phone"></div></div><label>Notes</label><textarea id="cnotes" placeholder="Preferences or service notes"></textarea><div class="actions"><button class="btn" onclick="addCustomer()">Save customer</button></div>`)}
 ${card("Customer list",state.customers.length?`<div class="table-wrap"><table><tr><th>Customer</th><th>Phone</th><th>Orders</th><th>Total ordered</th><th>Notes / actions</th></tr>${state.customers.map(c=>{let os=state.orders.filter(o=>o.customer.toLowerCase()===c.name.toLowerCase());return `<tr><td><b>${esc(c.name)}</b></td><td>${esc(c.phone||"—")}</td><td>${os.length}</td><td>${money(os.reduce((s,o)=>s+Number(o.total),0))}</td><td>${esc(c.notes||"")}<div class="actions"><button class="btn small light" onclick="editCustomer('${c.id}')">Edit</button><button class="btn small red" onclick="deleteCustomer('${c.id}')">Delete</button></div></td></tr>`}).join("")}</table></div>`:`<div class="empty">No customers yet. Customers are also added automatically when you save an order.</div>`)}
 `;
}
function addCustomer(){let name=val("cname");if(!name){alert("Enter a customer name.");return}if(state.customers.some(c=>c.name.toLowerCase()===name.toLowerCase())){alert("This customer already exists.");return}state.customers.push({id:id("CUS"),name,phone:val("cphone"),notes:val("cnotes")});save();render()}
function editCustomer(cid){let c=state.customers.find(x=>x.id===cid);if(!c)return;let name=prompt("Customer name",c.name);if(name===null)return;if(!name.trim())return alert("Name cannot be blank.");let phone=prompt("Phone",c.phone||"");if(phone===null)return;let notes=prompt("Notes",c.notes||"");if(notes===null)return;c.name=name.trim();c.phone=phone.trim();c.notes=notes.trim();save();render()}
function deleteCustomer(cid){if(confirm("Delete this customer record? Orders will remain.")){state.customers=state.customers.filter(c=>c.id!==cid);save();render()}}
function pricingPage(){
 return `<h2>Service pricing</h2><p class="sub">Update your service rates here. Rates are examples from your previous price list; verify before advertising.</p>
 ${card("Add a service",`<div class="row"><div><label>Service name</label><input id="pname" placeholder="e.g. Saree dry cleaning"></div><div><label>Rate (₹)</label><input id="prate" type="number" min="0" step="0.01" placeholder="100"></div><div><label>Unit</label><select id="punit">${options(["kg","item","pair","piece"],"item")}</select></div></div><div class="actions"><button class="btn" onclick="addPrice()">＋ Add service</button></div>`)}
 ${card("Current rates",`<div class="table-wrap"><table><tr><th>Service</th><th>Rate</th><th>Unit</th><th>Actions</th></tr>${state.prices.map((p,i)=>`<tr><td>${esc(p.name)}</td><td>${money(p.rate)}</td><td>${esc(p.unit)}</td><td><div class="actions"><button class="btn small light" onclick="editPrice(${i})">Edit</button><button class="btn small red" onclick="deletePrice(${i})">Delete</button></div></td></tr>`).join("")}</table></div>`)}
 `;
}
function addPrice(){let name=val("pname"),rate=num("prate"),unit=val("punit");if(!name||rate<=0){alert("Enter service name and a rate above zero.");return}if(state.prices.some(p=>p.name.toLowerCase()===name.toLowerCase())){alert("That service name already exists.");return}state.prices.push({name,rate,unit});save();render()}
function editPrice(i){let p=state.prices[i];let n=prompt("Service name",p.name);if(n===null)return;let r=prompt("Rate in rupees",p.rate);if(r===null)return;let u=prompt("Unit: kg, item, pair or piece",p.unit);if(u===null)return;if(!n.trim()||Number(r)<0||!Number.isFinite(Number(r)))return alert("Invalid details.");state.prices[i]={name:n.trim(),rate:Number(r),unit:u.trim()||"item"};save();render()}
function deletePrice(i){if(confirm("Delete this service from pricing? Existing orders will remain unchanged.")){state.prices.splice(i,1);save();render()}}
function invoicePage(){
 return `<h2>Invoice generator</h2><p class="sub">Create a simple invoice from an order or enter a manual invoice.</p>
 ${card("Invoice details",`<div class="row"><div><label>Choose saved order</label><select id="iselect" onchange="fillInvoice()"><option value="">— Manual invoice —</option>${state.orders.map(o=>`<option value="${o.id}">${esc(o.id)} · ${esc(o.customer)} · ${money(o.total)}</option>`).join("")}</select></div><div><label>Invoice number</label><input id="inum" value="INV-${today().replaceAll("-","")}"></div><div><label>Customer / hotel name</label><input id="icustomer" placeholder="Customer name"></div><div><label>Phone</label><input id="iphone" placeholder="Optional"></div><div><label>Service description</label><input id="idesc" placeholder="Laundry service"></div><div><label>Quantity</label><input id="iqty" type="number" min="0" step="0.1" value="1" oninput="calcInvoice()"></div><div><label>Rate (₹)</label><input id="irate" type="number" min="0" step="0.01" value="160" oninput="calcInvoice()"></div><div><label>Discount (₹)</label><input id="idiscount" type="number" min="0" value="0" oninput="calcInvoice()"></div></div><hr><div class="grid"><div><div class="kpi-label">Subtotal</div><div class="kpi" id="isub">₹160</div></div><div><div class="kpi-label">Total due</div><div class="kpi" id="itotal">₹160</div></div></div><div class="actions no-print"><button class="btn" onclick="printInvoice()">🖨 Print / Save PDF</button><button class="btn light" onclick="downloadInvoice()">Download invoice text</button></div>`)}
 <div id="invoicePaper" class="card"><div id="invoiceOutput"><div class="empty">Enter invoice details above, then print or download.</div></div></div>`;
}
function fillInvoice(){let o=state.orders.find(x=>x.id===val("iselect"));if(!o){calcInvoice();return}document.getElementById("icustomer").value=o.customer;document.getElementById("iphone").value=o.phone||"";document.getElementById("idesc").value=o.service;document.getElementById("iqty").value=o.qty;document.getElementById("irate").value=o.rate;calcInvoice()}
function invoiceData(){let qty=num("iqty"),rate=num("irate"),discount=num("idiscount"),subtotal=qty*rate,total=Math.max(0,subtotal-discount);return {number:val("inum")||"INV-"+Date.now(),customer:val("icustomer")||"Customer",phone:val("iphone"),desc:val("idesc")||"Laundry service",qty,rate,discount,subtotal,total,date:today()}}
function calcInvoice(){let d=invoiceData();let sub=document.getElementById("isub"),tot=document.getElementById("itotal"),out=document.getElementById("invoiceOutput");if(sub)sub.textContent=money(d.subtotal);if(tot)tot.textContent=money(d.total);if(out)out.innerHTML=`<h2>${esc(state.settings.business)}</h2><p>${esc(state.settings.address)}<br>${esc(state.settings.phone)}</p><hr><h3>INVOICE ${esc(d.number)}</h3><p>Date: ${d.date}</p><p><b>Bill to:</b> ${esc(d.customer)} ${d.phone?"· "+esc(d.phone):""}</p><div class="table-wrap"><table><tr><th>Description</th><th>Qty</th><th>Rate</th><th>Amount</th></tr><tr><td>${esc(d.desc)}</td><td>${d.qty}</td><td>${money(d.rate)}</td><td>${money(d.subtotal)}</td></tr></table></div><p style="text-align:right">Subtotal: <b>${money(d.subtotal)}</b><br>Discount: ${money(d.discount)}<br><strong style="font-size:22px">Total: ${money(d.total)}</strong></p><p>Thank you for choosing E-Wash Laundry — Your Clothes, Our Care.</p><small>Generated using E-Wash Laundry Business Manager.</small>`}
function printInvoice(){calcInvoice();window.print()}
function downloadInvoice(){calcInvoice();let d=invoiceData();downloadText("invoice-"+d.number+".txt",document.getElementById("invoiceOutput").innerText)}
function marketingPage(){
 const drafts=[
 ["Fresh laundry","Your clothes deserve the best care! ✨ Let E-Wash Laundry take care of your everyday laundry needs. Contact us to ask about our current services and rates. #EWashLaundry #LaundryCare #Powai"],
 ["Service spotlight","Busy week? Let laundry be one less thing to worry about. E-Wash Laundry is here for your clothing-care needs in Powai. Message us to check available services. 💙 #YourClothesOurCare"],
 ["Customer appreciation","Thank you for supporting E-Wash Laundry! Every order motivates us to keep caring for your clothes with attention and care. 💙 #EWashLaundry #Powai"],
 ["Special-care reminder","Some garments need extra attention. Ask E-Wash Laundry about special-care options for selected garments before placing your order. 🧺 #GarmentCare"]
 ];
 return `<h2>Content Studio</h2><p class="sub">Create drafts for Google Business Profile or social media. Review every claim before publishing.</p>
 ${card("Generate a post draft",`<label>Post type</label><select id="posttype">${drafts.map((d,i)=>`<option value="${i}">${esc(d[0])}</option>`).join("")}<option value="custom">Custom post</option></select><label>Extra message (optional)</label><textarea id="postextra" placeholder="Add a promotion, service or announcement. Avoid unverified offers."></textarea><div class="actions"><button class="btn" onclick="makePost()">Generate draft</button></div><label>Draft text</label><textarea id="postdraft" placeholder="Your draft will appear here. Edit it before use."></textarea><div class="actions"><button class="btn green" onclick="savePost()">Save draft</button><button class="btn light" onclick="copyPost()">Copy text</button><button class="btn light" onclick="downloadText('ewash-post.txt',val('postdraft'))">Download text</button></div><p><span class="pill yellow">Manual publishing only</span> No post will be published automatically.</p>`)}
 ${card("Saved drafts",state.posts.length?[...state.posts].reverse().map(p=>`<div class="item"><b>${esc(p.title)}</b><br><small>${esc(p.date)} · ${esc(p.status)}</small><p style="white-space:pre-wrap">${esc(p.text)}</p><div class="actions"><button class="btn small light" onclick="loadPost('${p.id}')">Edit / reuse</button><button class="btn small red" onclick="deletePost('${p.id}')">Delete</button></div></div>`).join(""):`<div class="empty">No saved drafts yet.</div>`)}
 `;
}
function makePost(){let type=val("posttype"),extra=val("postextra"),drafts=[
"Your clothes deserve the best care! ✨ Let E-Wash Laundry take care of your everyday laundry needs. Contact us to ask about our current services and rates. #EWashLaundry #LaundryCare #Powai",
"Busy week? Let laundry be one less thing to worry about. E-Wash Laundry is here for your clothing-care needs in Powai. Message us to check available services. 💙 #YourClothesOurCare",
"Thank you for supporting E-Wash Laundry! Every order motivates us to keep caring for your clothes with attention and care. 💙 #EWashLaundry #Powai",
"Some garments need extra attention. Ask E-Wash Laundry about special-care options for selected garments before placing your order. 🧺 #GarmentCare"
];let base=type==="custom"?"E-Wash Laundry — Your Clothes, Our Care. Contact us to ask about our laundry services and current rates.":drafts[Number(type)]||drafts[0];document.getElementById("postdraft").value=base+(extra?"\n\n"+extra:"")}
function savePost(){let text=val("postdraft");if(!text){alert("Generate or write a draft first.");return}state.posts.push({id:id("POST"),title:val("posttype")==="custom"?"Custom post":document.getElementById("posttype").selectedOptions[0].text,text,date:today(),status:"Draft"});save();render()}
function copyPost(){let t=val("postdraft");if(!t)return alert("Write a draft first.");if(navigator.clipboard&&navigator.clipboard.writeText)navigator.clipboard.writeText(t).then(()=>alert("Copied.")).catch(()=>prompt("Copy this text:",t));else prompt("Copy this text:",t)}
function loadPost(pid){let p=state.posts.find(x=>x.id===pid);if(!p)return;setPage("marketing");setTimeout(()=>{document.getElementById("postdraft").value=p.text},0)}
function deletePost(pid){if(confirm("Delete this draft?")){state.posts=state.posts.filter(p=>p.id!==pid);save();render()}}
function reviewsPage(){
 return `<h2>Review tracker</h2><p class="sub">Record review feedback manually. This does not fetch live Google reviews.</p>
 ${card("Add review record",`<div class="row"><div><label>Customer name (optional)</label><input id="rname" placeholder="Customer name"></div><div><label>Rating</label><select id="rrating">${options(["5","4","3","2","1"],"5")}</select></div></div><label>Review / feedback text</label><textarea id="rtext" placeholder="Paste feedback you are authorised to store"></textarea><div class="actions"><button class="btn" onclick="addReview()">Save review record</button><a class="btn light" href="${esc(state.settings.reviewLink)}" target="_blank" rel="noopener">Open Google review link ↗</a></div>`)}
 ${card("Saved feedback",state.reviews.length?[...state.reviews].reverse().map(r=>`<div class="item"><b>${"⭐".repeat(Number(r.rating))} ${esc(r.name||"Customer")}</b><small> · ${esc(r.date)}</small><p>${esc(r.text||"No written comment")}</p><button class="btn small red" onclick="deleteReview('${r.id}')">Delete</button></div>`).join(""):`<div class="empty">No review records yet.</div>`)}
 `;
}
function addReview(){state.reviews.push({id:id("REV"),name:val("rname"),rating:val("rrating"),text:val("rtext"),date:today()});save();render()}
function deleteReview(rid){if(confirm("Delete this review record?")){state.reviews=state.reviews.filter(r=>r.id!==rid);save();render()}}
function inboxPage(){
 return `<h2>Enquiries / WhatsApp tracker</h2><p class="sub">Manually record calls, WhatsApp enquiries and follow-ups. This does not read your WhatsApp messages.</p>
 ${card("Add enquiry",`<div class="row"><div><label>Customer name</label><input id="ename" placeholder="Name"></div><div><label>Contact number</label><input id="ephone" placeholder="Optional"></div><div><label>Channel</label><select id="echannel">${options(["WhatsApp","Phone call","Website","Walk-in","Other"],"WhatsApp")}</select></div><div><label>Status</label><select id="estatus">${options(["New","Follow-up needed","Converted to order","Closed"],"New")}</select></div></div><label>Enquiry details</label><textarea id="edetails" placeholder="Requested service, quote, follow-up date (avoid sensitive details)"></textarea><div class="actions"><button class="btn" onclick="addEnquiry()">Save enquiry</button><a class="btn green" href="https://wa.me/919867542518" target="_blank" rel="noopener">Open WhatsApp ↗</a></div>`)}
 ${card("Enquiry list",state.enquiries.length?`<div class="table-wrap"><table><tr><th>Date / customer</th><th>Channel</th><th>Details</th><th>Status</th><th>Action</th></tr>${[...state.enquiries].reverse().map(e=>`<tr><td>${esc(e.date)}<br><b>${esc(e.name||"Customer")}</b><br>${esc(e.phone||"")}</td><td>${esc(e.channel)}</td><td>${esc(e.details)}</td><td><select onchange="updateEnquiry('${e.id}',this.value)">${options(["New","Follow-up needed","Converted to order","Closed"],e.status)}</select></td><td><button class="btn small red" onclick="deleteEnquiry('${e.id}')">Delete</button></td></tr>`).join("")}</table></div>`:`<div class="empty">No enquiries recorded.</div>`)}
 `;
}
function addEnquiry(){state.enquiries.push({id:id("ENQ"),name:val("ename"),phone:val("ephone"),channel:val("echannel"),status:val("estatus"),details:val("edetails"),date:today()});save();render()}
function updateEnquiry(eid,status){let e=state.enquiries.find(x=>x.id===eid);if(e){e.status=status;save();render()}}
function deleteEnquiry(eid){if(confirm("Delete this enquiry?")){state.enquiries=state.enquiries.filter(e=>e.id!==eid);save();render()}}
function analyticsPage(){
 let m=state.metrics,rev=state.orders.filter(o=>o.payment==="Paid").reduce((s,o)=>s+Number(o.total),0);
 return `<h2>Analytics</h2><p class="sub">Enter figures from your Google Business Profile manually. Blank or zero values are not live Google data.</p>
 ${card("Google Business Profile metrics",`<div class="row"><div><label>Profile views</label><input id="mviews" type="number" min="0" value="${m.views}"></div><div><label>Searches</label><input id="msearches" type="number" min="0" value="${m.searches}"></div><div><label>Interactions</label><input id="minteractions" type="number" min="0" value="${m.interactions}"></div><div><label>Calls</label><input id="mcalls" type="number" min="0" value="${m.calls}"></div><div><label>Direction requests</label><input id="mdirections" type="number" min="0" value="${m.directions}"></div><div><label>Website clicks</label><input id="mwebsite" type="number" min="0" value="${m.website}"></div></div><div class="actions"><button class="btn" onclick="saveMetrics()">Save metrics</button></div><p><span class="pill yellow">Manually entered · Not live synced</span></p>`)}
 <div class="grid">${kpi("Orders",state.orders.length,"🧺")}${kpi("Paid revenue",money(rev),"💰")}${kpi("Customers",state.customers.length,"👥")}${kpi("Saved drafts",state.posts.length,"📣")}</div>
 ${card("Order status breakdown",statusBars())}
 ${card("Important limitation",`GitHub Pages hosts static files; it cannot securely run a backend or keep your private API keys. Live Google profile metrics, review syncing, WhatsApp inbox syncing, shared logins and cross-device data require additional secure services and official API setup.`)}
 `;
}
function saveMetrics(){state.metrics={views:num("mviews"),searches:num("msearches"),interactions:num("minteractions"),calls:num("mcalls"),directions:num("mdirections"),website:num("mwebsite")};save();render();alert("Manual metrics saved on this device.")}
function settingsPage(){
 return `<h2>Business settings</h2><p class="sub">Update your contact information and business links.</p>
 ${card("Business details",`<label>Business name</label><input id="sbusiness" value="${esc(state.settings.business)}"><label>Business phone</label><input id="sphone" value="${esc(state.settings.phone)}"><label>Business address</label><input id="saddress" value="${esc(state.settings.address)}"><label>Website</label><input id="swebsite" value="${esc(state.settings.website)}"><label>Google review link</label><input id="sreview" value="${esc(state.settings.reviewLink)}"><div class="actions"><button class="btn" onclick="saveSettings()">Save details</button></div>`)}
 ${card("Integration status",`<p><b>Google Business Profile:</b> ${pill("Not connected","yellow")}</p><p><b>WhatsApp Business Platform:</b> ${pill("Not connected","yellow")}</p><p><b>AI post generation:</b> ${pill("Built-in templates only","yellow")}</p><p><b>Data storage:</b> ${pill("This browser / device","yellow")}</p><hr><p>Never put Google OAuth secrets, Meta access tokens, passwords or customer private records into this public repository. A backend and proper authentication are required for secure integrations.</p>`)}
 ${card("Data management",`<p>Export a JSON backup before changing devices or clearing browser data. Restoring a backup replaces the records currently stored in this browser.</p><div class="actions"><button class="btn" onclick="exportBackup()">Download JSON backup</button><button class="btn light" onclick="exportCSV()">Export orders CSV</button><button class="btn red" onclick="clearAll()">Clear all app data</button></div>`)}
 `;
}
function saveSettings(){state.settings={business:val("sbusiness")||"E-Wash Laundry",phone:val("sphone"),address:val("saddress"),website:val("swebsite"),reviewLink:val("sreview")};save();render();alert("Business details saved.")}
function exportBackup(){downloadText("ewash-backup-"+today()+".json",JSON.stringify(state,null,2),"application/json")}
function restoreBackup(event){let file=event.target.files[0];if(!file)return;let reader=new FileReader();reader.onload=()=>{try{let d=JSON.parse(reader.result);if(!d.settings||!Array.isArray(d.orders)||!Array.isArray(d.prices))throw Error("Invalid backup");if(!confirm("Restore this backup and replace the current data on this device?"))return;state=Object.assign(structuredClone(defaultState),d);save();render();alert("Backup restored.")}catch(e){alert("Could not restore this file. Please select a valid E-Wash JSON backup.")}event.target.value=""};reader.readAsText(file)}
function exportCSV(){const rows=[["Order ID","Date","Customer","Phone","Service","Quantity","Rate","Total","Status","Payment"],...state.orders.map(o=>[o.id,o.date,o.customer,o.phone,o.service,o.qty,o.rate,o.total,o.status,o.payment])];const csv=rows.map(r=>r.map(v=>'"'+String(v??"").replace(/"/g,'""')+'"').join(",")).join("\r\n");downloadText("ewash-orders-"+today()+".csv",csv,"text/csv;charset=utf-8")}
function downloadText(filename,text,mime="text/plain;charset=utf-8"){const blob=new Blob([text],{type:mime});const url=URL.createObjectURL(blob);const a=document.createElement("a");a.href=url;a.download=filename;document.body.appendChild(a);a.click();a.remove();URL.revokeObjectURL(url)}
function clearAll(){if(confirm("This permanently clears this app's locally stored data on this device. Export a backup first. Continue?")){localStorage.removeItem(KEY);state=structuredClone(defaultState);save();render()}}
function useInvoice(orderId){setPage("invoice");setTimeout(()=>{document.getElementById("iselect").value=orderId;fillInvoice()},0)}
render();
</script>
</body>
</html>
