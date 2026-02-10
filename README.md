<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">

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
 height:100%;
 overflow-x:hidden;
 -webkit-text-size-adjust:100%;
}

body{
 margin:0;
 background:linear-gradient(135deg,#ffe4ec,#fff1f6);
 display:flex;
 justify-content:center;
 align-items:center;
 padding-bottom:env(safe-area-inset-bottom);
 touch-action:manipulation;
}

/* CARD */
.wrap{
 width:100%;
 max-width:420px;
 max-height:95vh;
 overflow-y:auto;
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

<audio id="clickSound">
<source src="https://assets.mixkit.co/sfx/preview/mixkit-select-click-1109.mp3">
</audio>

<script>
/* BLOCK ZOOM */
document.addEventListener('gesturestart',e=>e.preventDefault());
document.addEventListener('touchmove',e=>{
 if(e.scale!==1) e.preventDefault();
},{passive:false});
let lastTouch=0;
document.addEventListener('touchend',e=>{
 let now=Date.now();
 if(now-lastTouch<=300) e.preventDefault();
 lastTouch=now;
});

/* API URL ของ Google Sheet Web App */
const API_URL = "https://script.google.com/macros/s/AKfycbzNRFBiH4gN6Kog9jZ4672L2EzQONFwV_LhXZM9KbIg8KiMXns8OKP2NtuCucbrDwWIcw/exec";

/* APP */
let expenses=[];
const titleEl=title,amountEl=amount,payerEl=payer;
const listEl=list,summaryEl=finalSummary;
const toast=document.getElementById("toast");
const clickSound=document.getElementById("clickSound");

function showToast(m){
 toast.innerText=m;
 toast.classList.add("show");
 setTimeout(()=>toast.classList.remove("show"),2000);
}

function playClick(){
 clickSound.currentTime=0;
 clickSound.play();
}

function spawnHeart(){
 let h=document.createElement("div");
 h.className="heart";
 h.innerText="💖";
 document.body.appendChild(h);
 setTimeout(()=>h.remove(),1200);
}

function saveLocal(){
 localStorage.setItem("coupleData",JSON.stringify(expenses));
}
function loadLocal(){
 expenses=JSON.parse(localStorage.getItem("coupleData")||"[]");
}

function render(){
 listEl.innerHTML="";
 let ton=0,pam=0;
 expenses.forEach((e,i)=>{
  let half=e.amount/2;
  e.payer==="ต้น"?pam+=half:ton+=half;
  let d=document.createElement("div");
  d.className="item";
  d.innerHTML=`
   <span>${e.payer} ซื้อ ${e.title} ${e.amount}฿</span>
   <div class="actions">
    <button class="edit" onclick="editItem(${i})">✏️</button>
    <button class="del" onclick="deleteItem(${i})">❌</button>
   </div>`;
  listEl.appendChild(d);
 });
 if(ton>pam) summaryEl.innerText=`ต้นติดแป๋ม ${(ton-pam).toFixed(2)} บาท`;
 else if(pam>ton) summaryEl.innerText=`แป๋มติดต้น ${(pam-ton).toFixed(2)} บาท`;
 else summaryEl.innerText="🎉 เสมอกัน";
}

addBtn.onclick = async () => {
 playClick(); spawnHeart();

 if(!titleEl.value || !amountEl.value){
  showToast("กรอกข้อมูลให้ครบ");
  return;
 }

 const item = {
  title: titleEl.value,
  amount: Number(amountEl.value),
  payer: payerEl.value,
  recorder: payerEl.value
 };

 // บันทึก Local
 expenses.push(item);
 saveLocal();
 render();

 // ส่งเข้า Google Sheet
 try {
   await fetch(API_URL, {
     method:"POST",
     body:JSON.stringify(item)
   });
   showToast("บันทึกลงชีตแล้ว");
 } catch(err) {
   console.error(err);
   showToast("เชื่อมชีตไม่สำเร็จ");
 }

 titleEl.value="";
 amountEl.value="";
};

window.editItem = i => {
 titleEl.value = expenses[i].title;
 amountEl.value = expenses[i].amount;
 payerEl.value = expenses[i].payer;
 expenses.splice(i,1);
 saveLocal(); render();
};

window.deleteItem = i => {
 if(confirm("ลบรายการนี้?")){
  expenses.splice(i,1);
  saveLocal(); render();
 }
};

function openBill(){
 let t=0,bill="";
 expenses.forEach(e=>{
  bill += `${e.title} - ${e.amount}฿ (${e.payer})\n`;
  t += e.amount;
 });
 alert(bill + "\nรวม " + t + " บาท");
}

resetAll.onclick = () => {
 if(confirm("ล้างทั้งหมด?")){
  expenses = [];
  saveLocal(); render();
 }
};

loadLocal(); render();
</script>

</body>
</html>
