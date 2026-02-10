<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">

<!-- LOCK ZOOM -->
<meta name="viewport" 
content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

<title>แบบบันทึกรายจ่าย Cute</title>

<link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&display=swap" rel="stylesheet">

<style>
*{
 box-sizing:border-box;
 font-family:'Quicksand',sans-serif;
}

html,body{
 width:100%;
 overflow-x:hidden;
 -webkit-text-size-adjust:100%;
}

body{
 margin:0;
 min-height:100vh;
 background:linear-gradient(135deg,#ffe4ec,#fff1f6);
 display:flex;
 justify-content:center;
 padding-bottom:env(safe-area-inset-bottom);
}

/* CARD */
.wrap{
 width:100%;
 max-width:420px;
 margin:12px;
 background:white;
 border-radius:30px;
 padding:22px;
 box-shadow:0 10px 25px rgba(0,0,0,.12);
}

/* HEADER */
header{text-align:center;}
header h2{
 margin:5px 0 10px;
 font-size:30px;
 color:#ff6b8a;
}

/* SUMMARY */
.summary{
 background:#fff5f8;
 padding:14px;
 border-radius:18px;
 text-align:center;
 font-size:20px;
 font-weight:600;
 color:#ff4d6d;
 margin-bottom:14px;
}

/* INPUT */
input,select{
 width:100%;
 padding:14px;
 border-radius:16px;
 border:1px solid #ffd1dc;
 margin-bottom:10px;
 font-size:16px;
 outline:none;
}

/* BUTTON */
button{
 padding:14px;
 border-radius:16px;
 border:none;
 font-size:16px;
 font-weight:600;
 cursor:pointer;
 min-height:48px;
 transition:.2s;
}
button:hover{transform:translateY(-2px);}

.addBtn{background:#ff6b8a;color:white;}
.billBtn{background:#ffa3b8;color:white;}
.reset{
 background:#b2b2b2;
 color:white;
 margin-top:12px;
 width:100%;
}

/* GRID */
.btn-grid{
 display:grid;
 grid-template-columns:1fr 1fr;
 gap:10px;
 margin-top:6px;
}

/* ITEM */
.item{
 background:#fff5f8;
 padding:14px;
 border-radius:18px;
 margin-top:10px;
 display:flex;
 justify-content:space-between;
 align-items:center;
 animation:fade .3s;
}

@keyframes fade{
 from{opacity:0;transform:translateY(10px)}
 to{opacity:1;}
}

.actions button{
 padding:6px 10px;
 font-size:12px;
 border-radius:10px;
 margin-left:5px;
 min-height:auto;
}

.edit{background:#ffd56b;}
.del{background:#ff7b7b;color:white;}

/* MODAL */
.modal{
 position:fixed;
 inset:0;
 background:rgba(0,0,0,.4);
 display:none;
 justify-content:center;
 align-items:center;
}

.modal-content{
 background:white;
 width:90%;
 max-width:360px;
 padding:20px;
 border-radius:22px;
}

/* TOAST */
.toast{
 position:fixed;
 bottom:20px;
 left:50%;
 transform:translateX(-50%);
 background:#ff6b8a;
 color:white;
 padding:10px 20px;
 border-radius:20px;
 font-size:14px;
 opacity:0;
 transition:.3s;
}
.toast.show{opacity:1}

/* LOADING */
.loading{
 position:fixed;
 inset:0;
 background:rgba(0,0,0,.5);
 display:none;
 justify-content:center;
 align-items:center;
 color:white;
 font-size:22px;
 z-index:10;
}

/* HEART */
.heart{
 position:fixed;
 bottom:90px;
 left:50%;
 font-size:26px;
 animation:floatUp 1.2s ease-out forwards;
 pointer-events:none;
}
@keyframes floatUp{
 0%{transform:translate(-50%,0) scale(1);opacity:1;}
 100%{transform:translate(-50%,-130px) scale(1.8);opacity:0;}
}
</style>
</head>

<body>

<div class="loading" id="loading">⏳ กำลังโหลด...</div>
<div class="toast" id="toast"></div>

<div class="wrap">

<header>
<h2>💑 แบบบันทึกรายจ่าย 💑</h2>
</header>

<div class="summary" id="finalSummary">-</div>

<input id="title" placeholder="ชื่อรายการ">
<input id="amount" type="number" placeholder="จำนวนเงิน">

<select id="payer">
<option value="ต้น">ต้น ซื้อ</option>
<option value="แป๋ม">แป๋ม ซื้อ</option>
</select>

<div class="btn-grid">
<button class="addBtn" id="addBtn">➕ เพิ่ม</button>
<button class="billBtn" onclick="openBill()">🧾 บิล</button>
</div>

<div id="list"></div>

<button class="reset" id="resetAll">🗑 ล้างทั้งหมด</button>

</div>

<!-- BILL -->
<div class="modal" id="billModal">
 <div class="modal-content">
  <h3>🧾 บิลรวม</h3>
  <div id="billList"></div>
  <hr>
  <div id="billTotal"></div><br>
  <button class="reset" onclick="closeBill()">ปิด</button>
 </div>
</div>

<audio id="clickSound">
<source src="https://assets.mixkit.co/sfx/preview/mixkit-select-click-1109.mp3">
</audio>

<script>
let expenses=[];

const titleEl=title;
const amountEl=amount;
const payerEl=payer;
const listEl=list;
const summaryEl=finalSummary;
const toast=document.getElementById("toast");
const loading=document.getElementById("loading");
const clickSound=document.getElementById("clickSound");

/* UTIL */
function showToast(msg){
 toast.innerText=msg;
 toast.classList.add("show");
 setTimeout(()=>toast.classList.remove("show"),2000);
}

function playClick(){
 clickSound.currentTime=0;
 clickSound.play();
}

function spawnHeart(){
 const h=document.createElement("div");
 h.className="heart";
 h.innerText="💖";
 document.body.appendChild(h);
 setTimeout(()=>h.remove(),1200);
}

/* LOCAL STORAGE */
function saveLocal(){
 localStorage.setItem("coupleData",JSON.stringify(expenses));
}
function loadLocal(){
 expenses=JSON.parse(localStorage.getItem("coupleData")||"[]");
}

/* RENDER */
function render(){
 listEl.innerHTML="";
 let ton=0,pam=0;

 expenses.forEach((e,i)=>{
  const half=e.amount/2;
  e.payer==="ต้น"?pam+=half:ton+=half;

  const div=document.createElement("div");
  div.className="item";
  div.innerHTML=`
   <span>${e.payer} ซื้อ ${e.title} ${e.amount}฿</span>
   <div class="actions">
    <button class="edit" onclick="editItem(${i})">✏️</button>
    <button class="del" onclick="deleteItem(${i})">❌</button>
   </div>`;
  listEl.appendChild(div);
 });

 if(ton>pam) summaryEl.innerText=`ต้นติดแป๋ม ${(ton-pam).toFixed(2)} บาท`;
 else if(pam>ton) summaryEl.innerText=`แป๋มติดต้น ${(pam-ton).toFixed(2)} บาท`;
 else summaryEl.innerText="🎉 เสมอกัน";
}

/* ADD */
addBtn.onclick=()=>{
 playClick();
 spawnHeart();

 if(!titleEl.value||!amountEl.value){
  showToast("กรอกข้อมูลให้ครบ");
  return;
 }

 expenses.push({
  title:titleEl.value,
  amount:Number(amountEl.value),
  payer:payerEl.value
 });

 saveLocal();
 render();
 showToast("บันทึกแล้ว");

 titleEl.value="";
 amountEl.value="";
}

/* EDIT */
window.editItem=i=>{
 playClick();
 titleEl.value=expenses[i].title;
 amountEl.value=expenses[i].amount;
 payerEl.value=expenses[i].payer;
 expenses.splice(i,1);
 saveLocal();
 render();
}

/* DELETE */
window.deleteItem=i=>{
 playClick();
 if(confirm("ลบรายการนี้?")){
  expenses.splice(i,1);
  saveLocal();
  render();
 }
}

/* BILL */
function openBill(){
 playClick();
 let t=0;
 billList.innerHTML="";
 expenses.forEach(e=>{
  billList.innerHTML+=`${e.title} - ${e.amount}฿ (${e.payer})<br>`;
  t+=e.amount;
 });
 billTotal.innerHTML=`รวม ${t} บาท`;
 billModal.style.display="flex";
}
function closeBill(){
 playClick();
 billModal.style.display="none";
}

/* RESET */
resetAll.onclick=()=>{
 playClick();
 if(confirm("ล้างทั้งหมด?")){
  expenses=[];
  saveLocal();
  render();
 }
}

loadLocal();
render();
</script>

</body>
</html>
