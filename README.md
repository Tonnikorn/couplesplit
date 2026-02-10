<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CoupleSplit</title>

<style>
body{
  margin:0;
  background:linear-gradient(120deg,#ffe0f0,#fff0f5);
  font-family:sans-serif;
  display:flex; justify-content:center;
}
.wrap{
  width:100%; max-width:600px;
  background:#ffffffdd;
  margin:15px; border-radius:20px;
  padding:15px;
}
header{text-align:center;}
input,select,button{
  width:100%;
  padding:10px;
  margin:5px 0;
  border-radius:10px;
  border:1px solid #ffb6b9;
}
button{
  background:#ff6f91;
  color:white;
  border:none;
  font-weight:bold;
}
.item{
  background:#ffe4ec;
  padding:10px;
  margin:5px 0;
  border-radius:10px;
  display:flex;
  justify-content:space-between;
}
.summary{
  text-align:center;
  font-size:20px;
  margin:10px 0;
}
</style>
</head>

<body>

<div class="wrap">

<header>
<h2>CoupleSplit</h2>
<input id="youName" value="ต้น">
<input id="partnerName" value="แป๋ม">
</header>

<div class="summary" id="finalSummary">กำลังโหลด...</div>

<input id="title" placeholder="ชื่อรายการ">
<input id="amount" type="number" placeholder="จำนวนเงิน">
<select id="payer">
  <option value="you">คุณ</option>
  <option value="partner">แฟน</option>
</select>

<button id="addBtn">+ เพิ่มรายการ</button>

<div id="list"></div>

<button id="resetAll">รีเซ็ตทั้งหมด</button>

</div>

<script>
/* ===============================
   🔴 ใส่ URL Google Script ตรงนี้
================================= */
const SHEET_URL = "https://script.google.com/macros/s/AKfycbzNRFBiH4gN6Kog9jZ4672L2EzQONFwV_LhXZM9KbIg8KiMXns8OKP2NtuCucbrDwWIcw/exec";

let expenses=[];

/* ===============================
      โหลดข้อมูลจาก Google Sheet
================================= */
async function loadFromSheet(){
  const res = await fetch(SHEET_URL);
  expenses = await res.json();
  saveLocal();
  render();
}

/* ===============================
      LocalStorage
================================= */
function saveLocal(){
  localStorage.setItem("coupleData",JSON.stringify(expenses));
}
function loadLocal(){
  expenses = JSON.parse(localStorage.getItem("coupleData")||"[]");
}

/* ===============================
      แสดงผล
================================= */
function render(){
  list.innerHTML="";
  let youOwe=0, partnerOwe=0;

  expenses.forEach((e,i)=>{
    const half = e.amount/2;
    if(e.payer==="you") partnerOwe+=half;
    else youOwe+=half;

    list.innerHTML += `
      <div class="item">
        ${e.payerName} ซื้อ ${e.title} ${e.amount} บาท
      </div>
    `;
  });

  if(youOwe>partnerOwe)
    finalSummary.innerText=`คุณติดแฟน ${(youOwe-partnerOwe).toFixed(2)} บาท`;
  else if(partnerOwe>youOwe)
    finalSummary.innerText=`แฟนติดคุณ ${(partnerOwe-youOwe).toFixed(2)} บาท`;
  else
    finalSummary.innerText="🎉 เสมอกัน";
}

/* ===============================
      เพิ่มรายการ
================================= */
addBtn.onclick = async ()=>{
  if(!title.value || !amount.value){
    alert("กรอกข้อมูลให้ครบ");
    return;
  }

  const data={
    title:title.value,
    amount:Number(amount.value),
    payer:payer.value,
    payerName: payer.value==="you"
      ? youName.value
      : partnerName.value
  };

  // ส่งไป Google Sheet
  await fetch(SHEET_URL,{
    method:"POST",
    body:JSON.stringify(data)
  });

  title.value="";
  amount.value="";
  loadFromSheet();
}

/* ===============================
      รีเซ็ต
================================= */
resetAll.onclick=()=>{
  if(confirm("ล้างเฉพาะในเครื่อง?")){
    expenses=[];
    saveLocal();
    render();
  }
}

/* ===============================
      เริ่มต้น
================================= */
loadLocal();
render();
loadFromSheet();
</script>

</body>
</html>
