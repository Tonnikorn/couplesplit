<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>แบบบันทึกรายจ่าย Cute</title>
<link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&display=swap" rel="stylesheet">
<style>
  * { box-sizing: border-box; font-family: 'Quicksand', sans-serif; }
  html, body { width: 100%; height: 100%; margin: 0; overflow-x: hidden; -webkit-text-size-adjust: 100%; }
  body { background: linear-gradient(135deg, #ffe4ec, #fff1f6); display: flex; justify-content: center; align-items: center; padding-bottom: env(safe-area-inset-bottom); touch-action: manipulation; }
  .wrap { width: 100%; max-width: 420px; max-height: 95vh; overflow-y: auto; background: white; border-radius: 30px; padding: 22px; box-shadow: 0 10px 25px rgba(0,0,0,.12); margin: 10px; }
  header { text-align: center; }
  header h2 { margin: 5px 0 10px; font-size: 28px; color: #ff6b8a; }
  .summary { background: #fff5f8; padding: 14px; border-radius: 18px; text-align: center; font-size: 20px; font-weight: 600; color: #ff4d6d; margin-bottom: 14px; border: 2px dashed #ffd1dc; }
  input, select { width: 100%; padding: 14px; border-radius: 16px; border: 1px solid #ffd1dc; margin-bottom: 10px; font-size: 16px; outline: none; transition: .3s; }
  input:focus { border-color: #ff6b8a; box-shadow: 0 0 5px rgba(255,107,138,0.3); }
  button { padding: 14px; border-radius: 16px; border: none; font-size: 16px; font-weight: 600; cursor: pointer; min-height: 48px; transition: .2s; }
  button:active { transform: scale(0.95); }
  .addBtn { background: #ff6b8a; color: white; width: 100%; }
  .billBtn { background: #ffa3b8; color: white; width: 100%; }
  .btn-grid { display: grid; grid-template-columns: 2fr 1fr; gap: 10px; margin-top: 6px; }
  .reset { background: #e0e0e0; color: #777; margin-top: 15px; width: 100%; font-size: 12px; }
  .item { background: #fff5f8; padding: 12px 15px; border-radius: 18px; margin-top: 10px; display: flex; justify-content: space-between; align-items: center; animation: fade .3s; border-left: 5px solid #ff6b8a; }
  @keyframes fade { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; } }
  .item-info b { color: #ff6b8a; }
  .actions { display: flex; gap: 5px; }
  .actions button { padding: 8px; font-size: 14px; border-radius: 12px; min-height: auto; }
  .edit { background: #ffd56b; color: #5d4a1b; }
  .del { background: #ff7b7b; color: white; }
  .toast { position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%); background: rgba(255, 107, 138, 0.9); color: white; padding: 10px 25px; border-radius: 25px; font-size: 14px; opacity: 0; transition: .3s; z-index: 999; pointer-events: none; }
  .toast.show { opacity: 1; bottom: 50px; }
  .heart { position: fixed; pointer-events: none; font-size: 24px; animation: floatUp 1.2s ease-out forwards; z-index: 1000; }
  @keyframes floatUp { 0% { transform: translateY(0) scale(1); opacity: 1; } 100% { transform: translateY(-100px) scale(2); opacity: 0; } }
  .loading-overlay { display: none; text-align: center; color: #ff6b8a; font-size: 14px; margin-bottom: 10px; }
</style>
</head>
<body>

<div class="toast" id="toast">บันทึกเรียบร้อย 💖</div>

<div class="wrap">
  <header><h2>💑 บันทึกรักนักจ่าย 💑</h2></header>
  
  <div class="summary" id="finalSummary">กำลังโหลด...</div>
  <div id="loading" class="loading-overlay">อัปเดตข้อมูล...</div>

  <input id="title" placeholder="ซื้ออะไรดีนะ?">
  <input id="amount" type="number" inputmode="decimal" placeholder="จำนวนเงิน (บาท)">
  <select id="payer">
    <option value="ต้น">ต้น ซื้อ</option>
    <option value="แป๋ม">แป๋ม ซื้อ</option>
  </select>

  <div class="btn-grid">
    <button class="addBtn" id="addBtn">➕ เพิ่มรายการ</button>
    <button class="billBtn" onclick="openBill()">🧾 บิล</button>
  </div>

  <div id="list"></div>
  <button class="reset" id="resetAll">🗑 ล้างข้อมูลทั้งหมด</button>
</div>

<script>
const API_URL = "https://script.google.com/macros/s/AKfycbxU--bEN3k8zGOTLiROzlgIUpVqWAFkjCBDPNANZbv1Sf__R3g9i7OZzFNdOEbVDKRsDg/exec";
let expenses = [];
let isEditing = null;

const titleEl = document.getElementById("title");
const amountEl = document.getElementById("amount");
const payerEl = document.getElementById("payer");
const listEl = document.getElementById("list");
const summaryEl = document.getElementById("finalSummary");
const addBtn = document.getElementById("addBtn");

function showToast(m) {
  const t = document.getElementById("toast");
  t.innerText = m;
  t.classList.add("show");
  setTimeout(() => t.classList.remove("show"), 2500);
}

function createHeart(e) {
  const h = document.createElement("div");
  h.className = "heart";
  h.innerHTML = "💖";
  h.style.left = (e ? e.clientX : window.innerWidth / 2) + "px";
  h.style.top = (e ? e.clientY : window.innerHeight / 2) + "px";
  document.body.appendChild(h);
  setTimeout(() => h.remove(), 1200);
}

async function loadCloud(showLoad = false) {
  if(showLoad) document.getElementById("loading").style.display = "block";
  try {
    const res = await fetch(API_URL);
    expenses = await res.json();
    render();
  } catch (err) {
    console.error(err);
  }
  document.getElementById("loading").style.display = "none";
}

addBtn.onclick = async (e) => {
  if (!titleEl.value || !amountEl.value) {
    showToast("กรอกข้อมูลให้ครบก่อนนะจ๊ะ");
    return;
  }

  const data = {
    action: isEditing !== null ? "edit" : "add",
    title: titleEl.value,
    amount: Number(amountEl.value),
    payer: payerEl.value,
    index: isEditing
  };

  addBtn.disabled = true;
  addBtn.innerText = "รอแป๊บนะ...";

  await fetch(API_URL, { method: "POST", body: JSON.stringify(data) });

  titleEl.value = "";
  amountEl.value = "";
  isEditing = null;
  addBtn.innerText = "➕ เพิ่มรายการ";
  addBtn.disabled = false;
  
  createHeart(e);
  showToast(data.action === "edit" ? "แก้ไขแล้วน้า ✨" : "บันทึกแล้วจ้า 💖");
  loadCloud();
};

window.editItem = (i) => {
  const item = expenses[i];
  titleEl.value = item.title;
  amountEl.value = item.amount;
  payerEl.value = item.payer;
  isEditing = i;
  addBtn.innerText = "✅ ตกลงแก้ไข";
  window.scrollTo({ top: 0, behavior: 'smooth' });
};

window.deleteItem = async (i) => {
  if (confirm("ลบรายการนี้ใช่ไหม?")) {
    await fetch(API_URL, { method: "POST", body: JSON.stringify({ action: "delete", index: i }) });
    showToast("ลบเรียบร้อย 🗑️");
    loadCloud();
  }
};

document.getElementById("resetAll").onclick = async () => {
  if (confirm("ต้องการลบทั้งหมดเลยหรอ? ข้อมูลจะหายหมดเลยนะ!")) {
    await fetch(API_URL, { method: "POST", body: JSON.stringify({ action: "clear" }) });
    showToast("ล้างบ้านสะอาดกริ๊บ ✨");
    loadCloud();
  }
};

function render() {
  listEl.innerHTML = "";
  let tonHalf = 0, pamHalf = 0;

  expenses.forEach((e, i) => {
    let half = e.amount / 2;
    e.payer === "ต้น" ? pamHalf += half : tonHalf += half;

    listEl.innerHTML += `
      <div class="item">
        <div class="item-info">
          <b>${e.payer}</b> ซื้อ ${e.title}<br>
          <span>${e.amount.toLocaleString()} ฿</span>
        </div>
        <div class="actions">
          <button class="edit" onclick="editItem(${i})">✏️</button>
          <button class="del" onclick="deleteItem(${i})">❌</button>
        </div>
      </div>
    `;
  });

  if (tonHalf > pamHalf) summaryEl.innerText = `ต้นติดแป๋ม ${(tonHalf - pamHalf).toFixed(2)} บาท`;
  else if (pamHalf > tonHalf) summaryEl.innerText = `แป๋มติดต้น ${(pamHalf - tonHalf).toFixed(2)} บาท`;
  else summaryEl.innerText = "🎉 เสมอกันเป๊ะ!";
}

function openBill() {
  if(expenses.length === 0) return alert("ยังไม่มีข้อมูลจ้า");
  let t = 0, bill = "🧾 รายการทั้งหมด:\n\n";
  expenses.forEach(e => {
    bill += `• ${e.title} (${e.amount}฿) - ${e.payer}\n`;
    t += e.amount;
  });
  alert(bill + "\n💰 รวมยอดทั้งหมด: " + t + " บาท");
}

// โหลดครั้งแรก
loadCloud();
// ดึงข้อมูลใหม่ทุกๆ 20 วินาที (เพื่อให้ดู Real-time)
setInterval(() => loadCloud(true), 20000);
</script>
</body>
</html>
