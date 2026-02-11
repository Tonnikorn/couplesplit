<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Love Wallet 💑</title>
<link href="https://fonts.googleapis.com/css2?family=Mitr:wght@300;400;600&family=Quicksand:wght@500;700&display=swap" rel="stylesheet">
<style>
  :root {
    --primary: #ff85a2;
    --secondary: #ffb7c5;
    --bg: #fff5f7;
    --white: #ffffff;
    --ton: #4dabf7; /* สีฟ้าของต้น */
    --ton-soft: #e7f5ff;
    --pam: #ff94c2; /* สีชมพูของแป๋ม */
    --pam-soft: #fff0f6;
  }

  * { box-sizing: border-box; font-family: 'Mitr', sans-serif; -webkit-tap-highlight-color: transparent; }
  body { background: var(--bg); margin: 0; display: flex; justify-content: center; align-items: flex-start; min-height: 100vh; padding: 20px; color: #444; overflow-x: hidden; }
  
  .app-container { width: 100%; max-width: 450px; background: var(--white); border-radius: 40px; padding: 25px; box-shadow: 0 20px 40px rgba(255, 133, 162, 0.15); position: relative; }

  header { text-align: center; margin-bottom: 20px; }
  header h2 { font-family: 'Quicksand', sans-serif; color: var(--primary); margin: 0; font-size: 28px; letter-spacing: 1px; }
  header p { font-size: 13px; color: #aaa; margin: 5px 0 0; }

  .summary-card {
    background: linear-gradient(135deg, var(--primary), var(--secondary));
    padding: 25px; border-radius: 30px; text-align: center; color: white; margin-bottom: 25px;
    box-shadow: 0 10px 20px rgba(255, 133, 162, 0.3); transition: 0.5s;
  }
  .summary-card div { font-size: 26px; font-weight: 600; margin-top: 5px; }

  .input-group { background: #f9f9f9; padding: 20px; border-radius: 25px; margin-bottom: 20px; border: 1px solid #eee; }
  input, select {
    width: 100%; padding: 15px; border-radius: 15px; border: 2px solid #f0f0f0; 
    margin-bottom: 12px; font-size: 16px; outline: none; transition: 0.2s; background: white;
  }
  input:focus { border-color: var(--secondary); }

  .split-box { display: flex; gap: 10px; margin-bottom: 5px; }
  .split-option { flex: 1; position: relative; }
  .split-option input { position: absolute; opacity: 0; }
  .split-label {
    display: block; padding: 12px; background: white; border: 2px solid #f0f0f0;
    border-radius: 15px; text-align: center; font-size: 14px; cursor: pointer; transition: 0.3s; font-weight: 600;
  }
  .split-option input:checked + .split-label {
    background: var(--primary); color: white; border-color: var(--primary);
  }

  .add-btn { background: var(--primary); color: white; width: 100%; padding: 18px; border-radius: 18px; font-size: 18px; margin-top: 10px; box-shadow: 0 8px 15px rgba(255, 133, 162, 0.2); }
  .btn-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 10px; }
  .bill-btn { background: #eee; color: #666; padding: 12px; border-radius: 15px; }
  .clear-btn { background: #fff0f0; color: #ff7b7b; padding: 12px; border-radius: 15px; font-size: 12px; width: 100%; margin-top: 25px; }

  /* --- รายการแบบไฮไลต์ใหม่ --- */
  .list-container { margin-top: 25px; }
  .item {
    background: white; border-radius: 20px; padding: 15px 18px; margin-bottom: 15px;
    display: flex; justify-content: space-between; align-items: center;
    box-shadow: 0 4px 12px rgba(0,0,0,0.03); 
    border: 1px solid #f0f0f0;
    position: relative; overflow: hidden;
    animation: slideIn 0.4s ease forwards; opacity: 0;
  }

  /* แถบสีไฮไลต์ด้านข้างแยกคนจ่าย */
  .item.payer-ton { border-left: 8px solid var(--ton); background: linear-gradient(to right, var(--ton-soft), white 30%); }
  .item.payer-pam { border-left: 8px solid var(--pam); background: linear-gradient(to right, var(--pam-soft), white 30%); }

  @keyframes slideIn { from { opacity: 0; transform: translateX(-15px); } to { opacity: 1; transform: translateX(0); } }

  .item-left { display: flex; flex-direction: column; gap: 2px; }
  .item-title { font-weight: 600; font-size: 16px; color: #333; margin-bottom: 2px; }
  
  .payer-tag { font-size: 12px; font-weight: 600; display: flex; align-items: center; gap: 4px; margin-bottom: 3px; }
  .payer-tag.ton { color: var(--ton); }
  .payer-tag.pam { color: var(--pam); }

  .tag-split { font-size: 10px; background: #eee; color: #777; padding: 1px 6px; border-radius: 5px; font-weight: normal; margin-left: 5px; }

  .debt-text { font-size: 11px; font-weight: 600; margin-top: 2px; }
  .payer-ton .debt-text { color: #339af0; } /* แป๋มติดหนี้ต้น */
  .payer-pam .debt-text { color: #f06595; } /* ต้นติดหนี้แป๋ม */

  .item-right { text-align: right; }
  .item-price { font-size: 18px; font-weight: 700; color: #333; font-family: 'Quicksand', sans-serif; }

  .actions { margin-top: 8px; display: flex; gap: 8px; justify-content: flex-end; }
  .btn-icon { width: 32px; height: 32px; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 14px; cursor: pointer; }
  .edit-btn { background: #fff9db; color: #fab005; }
  .del-btn { background: #fff5f5; color: #ff6b6b; }

  #loader { position: fixed; top: 20px; right: 20px; display: none; background: white; padding: 8px 15px; border-radius: 20px; font-size: 11px; box-shadow: 0 5px 15px rgba(0,0,0,0.1); z-index: 100; border-left: 4px solid var(--primary); }
  .heart-pop { position: fixed; pointer-events: none; font-size: 24px; z-index: 1000; animation: heartFly 1s ease-out forwards; }
  @keyframes heartFly { 0% { transform: scale(0); opacity: 0; } 50% { opacity: 1; transform: scale(1.2); } 100% { transform: translateY(-80px) scale(1); opacity: 0; } }
</style>
</head>
<body>

<div id="loader">🔄 อัปเดตข้อมูล...</div>

<div class="app-container">
  <header>
    <h2>Love Wallet</h2>
    <p>💑 บันทึกรักนักจ่ายของ ต้น & แป๋ม</p>
  </header>

  <div class="summary-card" id="sumCard">
    <small id="summaryLabel">กำลังตรวจสอบยอดคงเหลือ...</small>
    <div id="summaryAmount">0.00 ฿</div>
  </div>

  <div class="input-group">
    <input id="title" placeholder="ซื้ออะไรมาเอ่ย? (เช่น หมูกระทะ)">
    <input id="amount" type="number" inputmode="decimal" placeholder="จำนวนเงิน (฿)">
    
    <select id="payer">
      <option value="ต้น">👨‍💼 ต้น เป็นคนจ่าย</option>
      <option value="แป๋ม">👩‍💼 แป๋ม เป็นคนจ่าย</option>
    </select>

    <div class="split-box">
      <label class="split-option">
        <input type="radio" name="splitType" value="half" checked>
        <span class="split-label">👥 หารครึ่ง</span>
      </label>
      <label class="split-option">
        <input type="radio" name="splitType" value="full">
        <span class="split-label">🎁 จ่ายให้ก่อน</span>
      </label>
    </div>

    <button class="add-btn" id="addBtn">บันทึกรายการ</button>
  </div>

  <div class="btn-grid">
    <button class="bill-btn" onclick="openBill()">🧾 ดูบิลรวม</button>
    <button class="bill-btn" onclick="loadCloud()">🔄 รีเฟรช</button>
  </div>

  <div id="list" class="list-container"></div>

  <button class="clear-btn" id="resetAll">🗑 ล้างข้อมูลทั้งหมดเพื่อเริ่มเดือนใหม่</button>
</div>

<script>
const API_URL = "https://script.google.com/macros/s/AKfycby5-K2T-7c_ZMImph3Fo3JBxUi7DNMucWgUKDr1BSGP2TKXYXhWI3E8-BEz0NyT9Sh_UA/exec";
let expenses = [];
let isEditing = null;

const titleEl = document.getElementById("title");
const amountEl = document.getElementById("amount");
const payerEl = document.getElementById("payer");
const listEl = document.getElementById("list");
const sumAmountEl = document.getElementById("summaryAmount");
const sumLabelEl = document.getElementById("summaryLabel");
const addBtn = document.getElementById("addBtn");

async function loadCloud(silent = false) {
  if(!silent) document.getElementById("loader").style.display = "block";
  try {
    const res = await fetch(API_URL);
    expenses = await res.json();
    render();
  } catch (e) { console.error(e); }
  document.getElementById("loader").style.display = "none";
}

function createHeart(e) {
  const h = document.createElement("div");
  h.className = "heart-pop"; h.innerHTML = "💖";
  h.style.left = (e.clientX || window.innerWidth/2) + "px";
  h.style.top = (e.clientY || window.innerHeight/2) + "px";
  document.body.appendChild(h);
  setTimeout(() => h.remove(), 1000);
}

addBtn.onclick = async (e) => {
  if (!titleEl.value || !amountEl.value) return alert("ใส่ข้อมูลให้ครบก่อนนะที่รัก");
  const splitType = document.querySelector('input[name="splitType"]:checked').value;
  const data = { action: isEditing !== null ? "edit" : "add", title: titleEl.value, amount: parseFloat(amountEl.value), payer: payerEl.value, splitType: splitType, index: isEditing };
  addBtn.disabled = true; addBtn.innerText = "กำลังจดลงสมุด...";
  await fetch(API_URL, { method: "POST", body: JSON.stringify(data) });
  titleEl.value = ""; amountEl.value = ""; isEditing = null;
  addBtn.innerText = "บันทึกรายการ"; addBtn.disabled = false;
  createHeart(e); loadCloud(true);
};

window.editItem = (i) => {
  const item = expenses[i];
  titleEl.value = item.title; amountEl.value = item.amount; payerEl.value = item.payer;
  document.querySelector(`input[name="splitType"][value="${item.splitType || 'half'}"]`).checked = true;
  isEditing = i; addBtn.innerText = "✅ บันทึกการแก้ไข";
  window.scrollTo({ top: 0, behavior: 'smooth' });
};

window.deleteItem = async (i) => {
  if (confirm("ลบรายการนี้ใช่ไหม?")) {
    await fetch(API_URL, { method: "POST", body: JSON.stringify({ action: "delete", index: i }) });
    loadCloud(true);
  }
};

document.getElementById("resetAll").onclick = async () => {
  if (confirm("ล้างข้อมูลทั้งหมดเลยหรอ? เริ่มต้นใหม่กันนะ!")) {
    await fetch(API_URL, { method: "POST", body: JSON.stringify({ action: "clear" }) });
    loadCloud();
  }
};

function render() {
  listEl.innerHTML = "";
  let tonDebtTotal = 0; let pamDebtTotal = 0;

  expenses.forEach((e, i) => {
    const amt = parseFloat(e.amount) || 0;
    const type = String(e.splitType).trim().toLowerCase();
    let debtForOther = (type === "full") ? amt : amt / 2;
    const isTon = e.payer === "ต้น";

    if (isTon) { pamDebtTotal += debtForOther; } else { tonDebtTotal += debtForOther; }

    listEl.innerHTML += `
      <div class="item ${isTon ? 'payer-ton' : 'payer-pam'}">
        <div class="item-left">
          <div class="payer-tag ${isTon ? 'ton' : 'pam'}">
            ${isTon ? '👨‍💼 ต้น เป็นคนจ่าย' : '👩‍💼 แป๋ม เป็นคนจ่าย'}
            <span class="tag-split">${type==='full'?'🎁 จ่ายเต็ม':'👥 หาร'}</span>
          </div>
          <div class="item-title">${e.title}</div>
          <div class="debt-text">
            ${isTon ? 'แป๋มต้องคืน' : 'ต้นต้องคืน'}: ${debtForOther.toLocaleString()} ฿
          </div>
        </div>
        <div class="item-right">
          <div class="item-price">${amt.toLocaleString()} ฿</div>
          <div class="actions">
            <div class="btn-icon edit-btn" onclick="editItem(${i})">✏️</div>
            <div class="btn-icon del-btn" onclick="deleteItem(${i})">🗑️</div>
          </div>
        </div>
      </div>
    `;
  });

  if (tonDebtTotal > pamDebtTotal) {
    sumLabelEl.innerText = "ต้น ต้องโอนคืนให้ แป๋ม";
    sumAmountEl.innerText = (tonDebtTotal - pamDebtTotal).toLocaleString(undefined, {minimumFractionDigits: 2}) + " ฿";
  } else if (pamDebtTotal > tonDebtTotal) {
    sumLabelEl.innerText = "แป๋ม ต้องโอนคืนให้ ต้น";
    sumAmountEl.innerText = (pamDebtTotal - tonDebtTotal).toLocaleString(undefined, {minimumFractionDigits: 2}) + " ฿";
  } else {
    sumLabelEl.innerText = "ยอดปัจจุบัน"; sumAmountEl.innerText = "เสมอกันเป๊ะ! 🎉";
  }
}

function openBill() {
  if(expenses.length === 0) return alert("ไม่มีข้อมูลจ้า");
  let t = 0; let bill = "🧾 รายการทั้งหมดเดือนนี้:\n\n";
  expenses.forEach(e => { bill += `• ${e.title}: ${e.amount}฿ (${e.payer})\n`; t += e.amount; });
  alert(bill + "\n💰 รวมใช้ไปทั้งสิ้น: " + t.toLocaleString() + " ฿");
}

loadCloud();
setInterval(() => loadCloud(true), 30000);
</script>
</body>
</html>
