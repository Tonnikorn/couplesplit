<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<title>Service Terminated - Love Wallet</title>
<link href="https://fonts.googleapis.com/css2?family=Mitr:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  :root {
    --danger: #e03131;
    --bg: #fff5f5;
  }

  body { 
    background: var(--bg); 
    margin: 0; 
    display: flex; 
    justify-content: center; 
    align-items: center;
    height: 100vh;
    font-family: 'Mitr', sans-serif;
    color: #444;
    text-align: center;
    padding: 20px;
  }

  .termination-card { 
    width: 100%; 
    max-width: 450px; 
    background: white; 
    border-radius: 30px; 
    padding: 40px 25px; 
    box-shadow: 0 15px 35px rgba(0,0,0,0.1);
    border-top: 10px solid var(--danger);
  }

  .icon {
    font-size: 60px;
    margin-bottom: 20px;
  }

  h1 {
    color: var(--danger);
    font-size: 28px;
    margin-bottom: 15px;
  }

  p {
    font-size: 18px;
    line-height: 1.6;
    color: #555;
    margin-bottom: 30px;
  }

  .status-badge {
    display: inline-block;
    padding: 8px 20px;
    background: #ffe3e3;
    color: var(--danger);
    border-radius: 50px;
    font-weight: 600;
    font-size: 14px;
    text-transform: uppercase;
  }

  .footer-note {
    font-size: 12px;
    color: #bbb;
    margin-top: 40px;
  }
</style>
</head>
<body>

<div class="termination-card">
  <div class="icon">🚫</div>
  <div class="status-badge">Closed Permanently</div>
  <h1>ยุติการให้บริการ</h1>
  <p>
    ผู้พัฒนาได้ทำการ<b>ปิดระบบนี้อย่างถาวร</b><br>
    เนื่องจากพบว่าข้อมูลบางส่วน<br>
    "ไม่มีความซื่อสัตย์" <br>
    เพียงพอสำหรับการดำเนินงานต่อ
  </p>
  
  <div class="footer-note">
    ระบบถูกระงับการเชื่อมต่อฐานข้อมูลและ API ทั้งหมดแล้ว
  </div>
</div>

<script>
  // Disable all functions
  window.onload = function() {
    console.log("Application Locked: Data Integrity Violation.");
    // Clear any previous local data if necessary
    localStorage.clear();
  };

  // Prevent any attempts to bypass via console
  window.loadCloud = () => { alert('การเข้าถึงถูกปฏิเสธ'); };
  window.openBill = () => { alert('ระบบปิดการใช้งาน'); };
</script>

</body>
</html>
