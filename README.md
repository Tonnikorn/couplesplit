<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CoupleSplit — ต้น & แป๋ม</title>
<style>
:root{
  --bg:#fff0f5; --card:#ffffffcc; --accent:#ff6f91; --accent2:#ffb6b9;
  --success:#22c55e; --danger:#ef4444; --text:#334155; --radius:20px;
  --font-family:'Comic Sans MS', cursive, sans-serif;
}
*{box-sizing:border-box;}
html, body{
  margin:0; padding:0; width:100%; min-height:100%;
  font-family:var(--font-family); background:var(--bg);
  display:flex; justify-content:center; align-items:flex-start;
  background-image: linear-gradient(120deg, #ffe0f0 0%, #fff0f5 100%);
}
.wrap{
  width:100%; max-width:600px; margin:20px; background:var(--card); 
  border-radius:var(--radius); backdrop-filter: blur(10px);
  box-shadow:0 10px 25px rgba(0,0,0,0.15); overflow:hidden; border:2px solid #ffe0f0;
  display:flex; flex-direction:column; animation:fadeIn 0.5s ease-in-out;
}
header{
  padding:20px; text-align:center; background:linear-gradient(90deg, #ff6f91, #ffb6b9); color:white;
  border-bottom-left-radius:var(--radius); border-bottom-right-radius:var(--radius);
}
header h1{margin:0; font-size:28px; letter-spacing:1px; text-shadow:1px 1px 4px rgba(0,0,0,0.3);}
header p{margin:5px 0 0 0; font-size:14px;}
.names{margin-top:10px; display:flex; justify-content:center; gap:10px; flex-wrap:wrap;}
.names input{
  padding:6px 12px; border-radius:12px; border:none; text-align:center; font-weight:600; font-size:14px;
  box-shadow:0 3px 6px rgba(0,0,0,0.1); min-width:80px;
}
.swap-btn{
  margin-top:10px; padding:6px 12px; border:none; border-radius:12px;
  background:#ffe0e6; color:var(--accent); cursor:pointer; font-weight:600; transition:0.3s;
  box-shadow:0 3px 6px rgba(0,0,0,0.15);
}
.swap-btn:hover{background:var(--accent2); color:white; transform:scale(1.05);}
main{
  padding:20px; flex:1; display:flex; flex-direction:column;
  overflow:auto;
  max-height:calc(100vh - 140px);
}
.summary{text-align:center; margin-bottom:20px;}
.summary div{margin:10px 0; font-size:18px; font-weight:600; padding:10px; border-radius:12px; background:#ffffff77; backdrop-filter: blur(5px);}
.summary .big{font-size:22px; font-weight:700;}
.list{margin-top:10px; flex:1; overflow-y:auto;}
.item{
  display:flex; justify-content:space-between; align-items:center;
  padding:10px; border-radius:12px; margin-bottom:8px; background:#fff3f6cc; 
  transition:0.2s; backdrop-filter: blur(5px); border:1px dashed #ffb6b9;
}
.item:hover{transform:scale(1.02); box-shadow:0 4px 10px rgba(0,0,0,0.1);}
.item button{
  padding:4px 10px; border:none; border-radius:10px; background:var(--accent2); color:white;
  font-weight:600; transition:0.2s;
}
.item button:hover{background:var(--accent); transform:scale(1.1);}
.add-card{display:flex; flex-direction:column; gap:10px; margin-bottom:20px;}
.add-card input, .add-card select{
  padding:8px 12px; border-radius:12px; border:1px solid #ffb6b9; font-size:14px;
  background:#ffffffaa; backdrop-filter: blur(5px);
}
.add-card input::placeholder{color:#888;}
.add-card button{
  padding:10px 15px; border:none; border-radius:12px; background:var(--accent); color:white; font-weight:700;
  transition:0.2s; cursor:pointer; box-shadow:0 4px 8px rgba(0,0,0,0.15);
}
.add-card button:hover{background:var(--accent2); transform:scale(1.05);}
footer{text-align:center; padding:10px; font-size:12px; color:#555;}

/* popup บิล */
#billPopup{
  display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.4);
  justify-content:center; align-items:center; z-index:1000;
  animation:fadeIn 0.3s;
}
#billContent{
  background:#fff; border-radius:20px; width:90%; max-width:500px; max-height:80%; overflow:auto; padding:20px; position:relative;
  box-shadow:0 10px 30px rgba(0,0,0,0.2); transform:scale(0.95); animation:popIn 0.3s forwards;
}
#billContent h2{color:#ff6f91; text-align:center; font-size:24px; margin-bottom:10px; animation:fadeColor 3s infinite alternate;}
#billContent p{font-size:18px; margin:10px 0;}
#billContent ul{list-style:none; padding:0;}
#billContent ul li{
  padding:5px 0; font-size:16px; color:#334155; background:#fff0f5aa; border-radius:10px; margin:3px 0;
  padding-left:10px; transition:0.2s;
}
#billContent ul li:hover{background:#ffe0e6aa; transform:scale(1.02);}
#billContent button.closePopup{
  position:absolute; top:10px; right:10px; background:#ff6f91; border:none; color:white; border-radius:50%; padding:8px 12px; cursor:pointer;
  font-size:18px; box-shadow:0 4px 10px rgba(0,0,0,0.2); transition:0.2s;
}
#billContent button.closePopup:hover{background:#ffb6b9; transform:scale(1.1);}

/* Animations */
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
@keyframes popIn{from{transform:scale(0.8);}to{transform:scale(1);}}
@keyframes fadeColor{
  0%{color:#ff6f91;}
  50%{color:#ff90a0;}
  100%{color:#ff6f91;}
}

/* Responsive scrollbar */
.list::-webkit-scrollbar, #billContent::-webkit-scrollbar{
  width:6px;
}
.list::-webkit-scrollbar-thumb, #billContent::-webkit-scrollbar-thumb{
  background:#ffb6b9; border-radius:3px;
}
</style>
</head>
<body>
<div class="wrap">
  <header>
    <h1>CoupleSplit</h1>
    <p>แบ่งจ่ายสำหรับคู่รัก น่ารัก สดใส</p>
    <div class="names">
      <input id="youName" value="ต้น" />
      <input id="partnerName" value="แป๋ม" />
    </div>
    <button class="swap-btn" id="swapNamesBtn">🔄 สลับชื่อ</button>
  </header>
  <main>
    <div class="summary">
      <div id="finalSummary" class="big">ยังไม่มีข้อมูล</div>
    </div>
    <div class="add-card">
      <div class="add-card">
  <!-- ลบ list="itemList" และ datalist ออก -->
  <input id="title" placeholder="ชื่อรายการ เช่น ข้าวเที่ยง">
  
  <input id="amount" placeholder="จำนวนเต็มก่อนหารครึ่ง (฿)" type="number" step="0.01">
  <select id="payer">
    <option value="you">คุณ</option>
    <option value="partner">แฟน</option>
  </select>
  <button id="addBtn">+ เพิ่มรายการ</button>
</div>

      <input id="amount" placeholder="จำนวนเต็มก่อนหารครึ่ง (฿)" type="number" step="0.01">
      <select id="payer">
        <option value="you">คุณ</option>
        <option value="partner">แฟน</option>
      </select>
      <button id="addBtn">+ เพิ่มรายการ</button>
    </div>
    <div class="list" id="list"></div>
    <div style="text-align:center; margin-top:20px;">
      <button id="viewBillBtn">📄 ดูบิลสรุป</button>
      <button id="resetAll">♻️ รีเซ็ตทั้งหมด</button>
    </div>
  </main>
  <footer>CoupleSplit — ใช้งานง่าย น่ารัก สดใส</footer>
</div>

<div id="billPopup">
  <div id="billContent">
    <button class="closePopup">❌</button>
    <div id="billHTML"></div>
  </div>
</div>

<script>
(function(){
  const $ = id=>document.getElementById(id);
  const youInput=$('youName'), partnerInput=$('partnerName');
  const finalSummary=$('finalSummary');
  const listEl=$('list'), addBtn=$('addBtn'), resetAll=$('resetAll'), viewBillBtn=$('viewBillBtn');
  const title=$('title'), amount=$('amount'), payer=$('payer');
  const swapBtn=$('swapNamesBtn');
  const billPopup=$('billPopup'), billHTML=$('billHTML'), closePopup=document.querySelector('.closePopup');
  const STORAGE_KEY='couplesplit:expenses';
  let expenses=[];

  function save(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(expenses)); }
  function load(){ expenses=JSON.parse(localStorage.getItem(STORAGE_KEY)||'[]'); render(); }
  function fmt(n){ return Number(n).toFixed(2)+' ฿'; }

  function computeBalances(list){
    let you=0, partner=0;
    list.forEach(e=>{
      const half=Number(e.amount)/2;
      if(e.payerName === youInput.value) partner+=half;
      else you+=half;
    });
    return {youOwes:you, partnerOwes:partner};
  }

  function render(){
    listEl.innerHTML='';
    if(expenses.length===0){ listEl.innerHTML='<div style="text-align:center; color:#888;">ยังไม่มีรายการ</div>'; }
    expenses.forEach((ex,i)=>{
      const item=document.createElement('div'); item.className='item';
      item.innerHTML=`<span>${ex.payerName} ซื้อ ${ex.title} ${Number(ex.amount).toFixed(2)} บาท</span>`;
      const delBtn=document.createElement('button'); delBtn.textContent='❌ ลบ';
      delBtn.onclick=()=>{expenses.splice(i,1); save(); render();}
      item.appendChild(delBtn); listEl.appendChild(item);
    });

    const balances=computeBalances(expenses);
    const youName=youInput.value, partnerName=partnerInput.value;
    if(balances.partnerOwes > balances.youOwes){
      finalSummary.innerHTML=`<span style="color:var(--success);">${partnerName}</span> ติด <span style="color:var(--danger);">${youName}</span> ${fmt(balances.partnerOwes - balances.youOwes)}`;
    } else if(balances.youOwes > balances.partnerOwes){
      finalSummary.innerHTML=`<span style="color:var(--danger);">${youName}</span> ติด <span style="color:var(--success);">${partnerName}</span> ${fmt(balances.youOwes - balances.partnerOwes)}`;
    } else{
      finalSummary.innerHTML=`🎉 เสมอกัน ไม่มีใครติดใคร`;
    }
  }

  addBtn.onclick=()=>{
    const t=title.value.trim(), a=parseFloat(amount.value);
    if(!t||!a||a<=0){alert('กรุณากรอกชื่อและจำนวนให้ถูกต้อง'); return;}
    const name = payer.value==='you' ? youInput.value : partnerInput.value;
    expenses.push({title:t, amount:a, payer:payer.value, payerName:name});
    title.value=''; amount.value='';
    save(); render();
  };

  resetAll.onclick=()=>{ if(confirm('ลบข้อมูลทั้งหมด?')){ expenses=[]; save(); render(); } };
  swapBtn.onclick=()=>{ const tmp=youInput.value; youInput.value=partnerInput.value; partnerInput.value=tmp; render(); };
  viewBillBtn.onclick=()=>{
    const youName=youInput.value, partnerName=partnerInput.value;
    const balances=computeBalances(expenses);
    let html=`<h2>📄 บิลสรุป CoupleSplit</h2>`;
    if(balances.partnerOwes > balances.youOwes){
      html+=`<p style="color:green; font-weight:600;">${partnerName} ติด ${youName}: ${fmt(balances.partnerOwes - balances.youOwes)}</p>`;
    } else if(balances.youOwes > balances.partnerOwes){
      html+=`<p style="color:red; font-weight:600;">${youName} ติด ${partnerName}: ${fmt(balances.youOwes - balances.partnerOwes)}</p>`;
    } else{ html+=`<p style="font-weight:600;">🎉 เสมอกัน ไม่มีใครติดใคร</p>`; }
    html+='<h3 style="margin-top:15px; text-align:center;">รายการทั้งหมด</h3><ul>';
    expenses.forEach(ex=>{ html+=`<li>${ex.payerName} ซื้อ ${ex.title} ${Number(ex.amount).toFixed(2)} บาท</li>`; });
    html+='</ul>';
    billHTML.innerHTML=html; billPopup.style.display='flex';
  };
  closePopup.onclick=()=>{ billPopup.style.display='none'; }
  load();
})();
</script>
</body>
</html>
