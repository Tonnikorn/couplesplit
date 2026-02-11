<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<title>Love Wallet 💑</title>
<link href="https://fonts.googleapis.com/css2?family=Mitr:wght@300;400;600&family=Quicksand:wght@500;700&display=swap" rel="stylesheet">
<style>
  :root {
    --primary: #ff85a2;
    --secondary: #ffb7c5;
    --bg: #fff5f7;
    --white: #ffffff;
    --ton: #4dabf7;
    --ton-soft: #e7f5ff;
    --pam: #ff94c2;
    --pam-soft: #fff0f6;
    --shadow: rgba(255, 133, 162, 0.15);
  }

  * { box-sizing: border-box; font-family: 'Mitr', sans-serif; -webkit-tap-highlight-color: transparent; }
  
  body { 
    background: var(--bg); 
    margin: 0; 
    display: flex; 
    justify-content: center; 
    padding: env(safe-area-inset-top) 12px env(safe-area-inset-bottom);
    color: #444;
  }

  .app-container { 
    width: 100%; 
    max-width: 480px; 
    background: var(--white); 
    border-radius: 35px; 
    padding: 20px 15px; 
    box-shadow: 0 10px 40px var(--shadow);
    margin-top: 10px;
  }

  header h2 { font-family: 'Quicksand', sans-serif; color: var(--primary); margin: 0; font-size: 24px; text-align: center; }
  header p { font-size: 12px; color: #bbb; margin: 4px 0 18px; text-align: center; font-weight: 300; }

  /* Summary Card */
  .summary-card {
    background: linear-gradient(135deg, var(--primary), var(--secondary));
    padding: 20px 15px; 
    border-radius: 22px; 
    text-align: center; 
    color: white; 
    margin-bottom: 20px;
    box-shadow: 0 8px 20px rgba(255, 133, 162, 0.3);
  }
  .summary-card small { font-size: 13px; opacity: 0.9; display: block; margin-bottom: 4px; font-weight: 300; }
  .summary-card div { font-size: 26px; font-weight: 700; font-family: 'Quicksand'; letter-spacing: 0.5px; }

  /* Input Section */
  .input-group { 
    background: #fdfdfd; 
    padding: 18px; 
    border-radius: 24px; 
    margin-bottom: 15px; 
    border: 1px solid #f1f1f1;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.02);
  }
  
  input, select {
    width: 100%; 
    padding: 14px; 
    border-radius: 14px; 
    border: 1.5px solid #eee; 
    margin-bottom: 10px; 
    font-size: 15px; 
    outline: none; 
    background: white;
    transition: border-color 0.2s;
  }
  input:focus { border-color: var(--primary); }

  .split-box { display: flex; gap: 10px; margin-bottom: 10px; }
  .split-option { flex: 1; position: relative; }
  .split-label {
    display: block; 
    padding: 12px; 
    background: white; 
    border: 1.5px solid #eee;
    border-radius: 14px; 
    text-align: center; 
    font-size: 14px; 
    cursor: pointer; 
    font-weight: 600;
    transition: 0.2s;
  }
  .split-option input { position: absolute; opacity: 0; }
  .split-option input:checked + .split-label { 
    background: var(--primary); 
    color: white; 
    border-color: var(--primary);
    box-shadow: 0 4px 10px rgba(255, 133, 162, 0.3);
  }

  .add-btn { 
    background: var(--primary); 
    color: white; 
    width: 100%; 
    padding: 16px; 
    border-radius: 16px; 
    font-size: 17px; 
    border: none; 
    font-weight: 600; 
    margin-top: 5px;
    box-shadow: 0 5px 15px rgba(255, 133, 162, 0.2);
    active { transform: scale(0.98); }
  }

  .btn-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 20px; }
  .bill-btn { 
    background: #f1f1f1; 
    color: #666; 
    padding: 12px; 
    border-radius: 14px; 
    font-size: 14px; 
    border: none; 
    font-weight: 400;
  }

  /* --- List Item --- */
  .item {
    background: white; 
    border-radius: 18px; 
    padding: 14px; 
    margin-bottom: 12px;
    display: flex; 
    justify-content: space-between; 
    align-items: center;
    box-shadow: 0 4px 12px rgba(0,0,0,0.03); 
    border: 1px solid #f8f8f8;
    position: relative; 
    animation: slideIn 0.3s ease-out;
  }
  .item.payer-ton { border-left: 6px solid var(--ton); background: linear-gradient(to right, var(--ton-soft), #fff 30%); }
  .item.payer-pam { border-left: 6px solid var(--pam); background: linear-gradient(to right, var(--pam-soft), #fff 30%); }

  .item-left { flex: 1; padding-right: 10px; min-width: 0; }
  .payer-tag { font-size: 11px; font-weight: 600; margin-bottom: 4px; display: flex; align-items: center; gap: 5px; }
  .payer-tag.ton { color: #2b80c5; }
  .payer-tag.pam { color: #d6336c; }
  .tag-split { background: rgba(0,0,0,0.06); color: #666; padding: 2px 6px; border-radius: 6px; font-weight: 400; font-size: 10px; }

  .item-title { font-weight: 600; font-size: 15px; color: #333; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .debt-text { font-size: 11px; font-weight: 400; margin-top: 2px; color: #777; }
  .debt-text b { color: #333; }

  .item-right { text-align: right; flex-shrink: 0; }
  .item-price { font-size: 18px; font-weight: 700; color: #333; font-family: 'Quicksand'; }

  .actions { margin-top: 8px; display: flex; gap: 8px; justify-content: flex-end; }
  .btn-icon { width: 32px; height: 32px; border-radius: 10px; display: flex; align-items: center; justify-content: center; font-size: 14px; cursor: pointer; }
  .edit-btn { background: #fff9db; color: #fab005; }
  .del-btn { background: #fff5f5; color: #ff6b6b; }

  .clear-btn { background: none; color: #ffb7b7; padding: 15px; font-size: 12px; width: 100%; border: none; margin-top: 10px; text-decoration: underline; opacity: 0.7; }

  #loader { 
    position: fixed; top: 20px; right: 20px; background: white; 
    padding: 8px 15px; border-radius: 25px; font-size: 11px; 
    box-shadow: 0 5px 15px rgba(0,0,0,0.1); z-index: 100; 
    border-left: 4px solid var(--primary); display: none; 
    font-weight: 600;
  }
  
  @keyframes slideIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
</style>
</head>
<body>

<div id="loader">🔄 กำลังอัปเดตข้อมูล...</div>

<div class="app-container">
  <header>
    <h2>Love Wallet 💖</h2>
    <p>บันทึกกระเป๋าตังค์ ต้น & แป๋ม</p>
  </header>

  <div class="summary-card" id="sumCard">
    <small id="summaryLabel">กำลังคำนวณยอดคงค้าง...</small>
    <div id="summaryAmount">0.00 ฿</div>
  </div>

  <div class="input-group">
    <input id="title" placeholder="ซื้ออะไรมาเอ่ย? (เช่น ค่าข้าว, ชานม)">
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
        <span class="split-label">🎁 จ่ายให้เต็ม</span>
      </label>
    </div>

    <button class="add-btn" id="addBtn">บันทึกรายการ</button>
  </div>

  <div class="btn-grid">
    <button class="bill-btn" onclick="openBill()">🧾 ดูสรุปบิล</button>
    <button class="bill-btn" onclick="loadCloud()">🔄 รีเฟรชข้อมูล</button>
  </div>

  <div id="list" class="list-container"></div>

  <button class="clear-btn" id="resetAll">ล้างข้อมูลทั้งหมด</button>
</div>

<script>
// สคริปต์การทำงานคงเดิมตาม Logic ของคุณ
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

addBtn.onclick = async () => {
  if (!titleEl.value || !amountEl.value) return alert("ใส่ข้อมูลให้ครบนะจ๊ะ");
  const splitType = document.querySelector('input[name="splitType"]:checked').value;
  const data = { action: isEditing !== null ? "edit" : "add", title: titleEl.value, amount: parseFloat(amountEl.value), payer: payerEl.value, splitType: splitType, index: isEditing };
  
  addBtn.disabled = true; addBtn.innerText = "กำลังบันทึก...";
  
  try {
    await fetch(API_URL, { method: "POST", body: JSON.stringify(data) });
    titleEl.value = ""; amountEl.value = ""; isEditing = null;
    addBtn.innerText = "บันทึกรายการ";
  } catch (e) { alert("เกิดข้อผิดพลาด ลองใหม่อีกครั้ง"); }
  
  addBtn.disabled = false;
  loadCloud(true);
};

window.editItem = (i) => {
  const item = expenses[i];
  titleEl.value = item.title; 
  amountEl.value = item.amount; 
  payerEl.value = item.payer;
  document.querySelector(`input[name="splitType"][value="${item.splitType || 'half'}"]`).checked = true;
  isEditing = i; 
  addBtn.innerText = "✅ บันทึกการแก้ไข";
  window.scrollTo({ top: 0, behavior: 'smooth' });
};

window.deleteItem = async (i) => {
  if (confirm("ลบรายการนี้ใช่ไหม?")) {
    await fetch(API_URL, { method: "POST", body: JSON.stringify({ action: "delete", index: i }) });
    loadCloud(true);
  }
};

document.getElementById("resetAll").onclick = async () => {
  if (confirm("ต้องการล้างข้อมูลทั้งหมดจริงหรือไม่?")) {
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
            <span>${isTon ? '👨‍💼 ต้น' : '👩‍💼 แป๋ม'}</span>
            <span class="tag-split">${type==='full'?'จ่ายเต็ม':'หารครึ่ง'}</span>
          </div>
          <div class="item-title">${e.title}</div>
          <div class="debt-text">
            ${isTon ? 'แป๋ม' : 'ต้น'}ต้องคืน: <b>${debtForOther.toLocaleString(undefined, {minimumFractionDigits: 2})} ฿</b>
          </div>
        </div>
        <div class="item-right">
          <div class="item-price">${amt.toLocaleString()}</div>
          <div class="actions">
            <div class="btn-icon edit-btn" onclick="editItem(${i})">✏️</div>
            <div class="btn-icon del-btn" onclick="deleteItem(${i})">🗑️</div>
          </div>
        </div>
      </div>
    `;
  });

  const diff = tonDebtTotal - pamDebtTotal;
  if (diff > 0) {
    sumLabelEl.innerText = "ต้น ต้องคืนให้ แป๋ม";
    sumAmountEl.innerText = diff.toLocaleString(undefined, {minimumFractionDigits: 2}) + " ฿";
  } else if (diff < 0) {
    sumLabelEl.innerText = "แป๋ม ต้องคืนให้ ต้น";
    sumAmountEl.innerText = Math.abs(diff).toLocaleString(undefined, {minimumFractionDigits: 2}) + " ฿";
  } else {
    sumLabelEl.innerText = "ยอดปัจจุบัน"; sumAmountEl.innerText = "เจ๊ากันพอดี! 🎉";
  }
}

function openBill() {
  if(expenses.length === 0) return alert("ไม่มีข้อมูลรายการ");
  let t = 0; let bill = "🧾 รายการทั้งหมด:\n------------------\n";
  expenses.forEach(e => { bill += `• ${e.title}: ${parseFloat(e.amount).toLocaleString()}฿\n`; t += parseFloat(e.amount); });
  alert(bill + "------------------\n💰 ยอดรวมทั้งสิ้น: " + t.toLocaleString() + " ฿");
}

loadCloud();
// Refresh อัตโนมัติทุก 30 วินาที
setInterval(() => loadCloud(true), 30000);
</script>
</body>
</html>
