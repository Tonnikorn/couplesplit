<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CoupleSplit</title>

<style>
body{
 font-family:Comic Sans MS;
 background:#fff0f5;
 display:flex;
 justify-content:center;
}
.wrap{
 background:#ffffffcc;
 width:100%;
 max-width:500px;
 padding:20px;
 border-radius:20px;
 box-shadow:0 0 15px rgba(0,0,0,.2);
}
header{
 text-align:center;
 background:#ff6f91;
 color:white;
 padding:15px;
 border-radius:15px;
}
.names input{
 width:45%;
 padding:6px;
 margin:5px;
 border-radius:10px;
 border:none;
}
.item{
 background:#ffe4ec;
 padding:8px;
 border-radius:10px;
 margin:5px 0;
 display:flex;
 justify-content:space-between;
}
button{
 background:#ff6f91;
 color:white;
 border:none;
 padding:8px;
 border-radius:10px;
 margin-top:5px;
 width:100%;
}
.add-card input,select{
 width:100%;
 padding:8px;
 border-radius:10px;
 border:1px solid pink;
 margin:5px 0;
}
</style>
</head>

<body>

<div class="wrap">

<header>
<h2>CoupleSplit</h2>
<div class="names">
<input id="youName" value="ต้น">
<input id="partnerName" value="แป๋ม">
</div>
<button id="swap">สลับชื่อ</button>
</header>

<h3 id="summary">ยังไม่มีข้อมูล</h3>

<div class="add-card">
<input id="title" placeholder="ชื่อรายการ">
<input id="amount" type="number" placeholder="จำนวนเงิน">
<select id="payer">
 <option value="you">คุณ</option>
 <option value="partner">แฟน</option>
</select>
<button id="addBtn">เพิ่มรายการ</button>
</div>

<div id="list"></div>

<button id="reset">รีเซ็ตทั้งหมด</button>

</div>

<script>

const API_URL="https://script.google.com/macros/s/AKfycbxcmKsJZMDVpc_QktPe_JjEwt8SVvJvIIVTXohjggenF3doLM7HQTCWB8MSb-1PhVHLOg/exec";

let expenses=[];

const youName=document.getElementById("youName");
const partnerName=document.getElementById("partnerName");
const title=document.getElementById("title");
const amount=document.getElementById("amount");
const payer=document.getElementById("payer");
const list=document.getElementById("list");
const summary=document.getElementById("summary");

function load(){
 fetch(API_URL)
 .then(r=>r.json())
 .then(data=>{
   expenses=data.map(r=>({
     title:r[0],
     amount:Number(r[1]),
     payer:r[2]
   }));
   render();
 });
}

function save(){
 fetch(API_URL,{
  method:"POST",
  body:JSON.stringify(expenses)
 });
}

function getName(p){
 return p==="you"?youName.value:partnerName.value;
}

function render(){
 list.innerHTML="";
 let you=0,partner=0;

 expenses.forEach((e,i)=>{
  const half=e.amount/2;
  if(e.payer==="you") partner+=half;
  else you+=half;

  const div=document.createElement("div");
  div.className="item";
  div.innerHTML=`${getName(e.payer)} ซื้อ ${e.title} ${e.amount} บาท`;
  const b=document.createElement("button");
  b.textContent="❌";
  b.onclick=()=>{
   expenses.splice(i,1);
   save();
   render();
  };
  div.appendChild(b);
  list.appendChild(div);
 });

 if(you>partner)
  summary.innerHTML=`${youName.value} ติด ${partnerName.value} ${(you-partner).toFixed(2)} บาท`;
 else if(partner>you)
  summary.innerHTML=`${partnerName.value} ติด ${youName.value} ${(partner-you).toFixed(2)} บาท`;
 else
  summary.innerHTML="🎉 เสมอกัน";
}

document.getElementById("addBtn").onclick=()=>{
 if(!title.value||!amount.value)
  return alert("กรอกข้อมูลให้ครบ");

 expenses.push({
  title:title.value,
  amount:Number(amount.value),
  payer:payer.value
 });

 title.value="";
 amount.value="";

 save();
 render();
};

document.getElementById("swap").onclick=()=>{
 [youName.value,partnerName.value]=
 [partnerName.value,youName.value];
 render();
};

document.getElementById("reset").onclick=()=>{
 if(confirm("ลบทั้งหมด?")){
  expenses=[];
  save();
  render();
 }
};

load();

</script>

</body>
</html>
