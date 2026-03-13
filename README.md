<!DOCTYPE html><html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Steel Weight Calculator</title>
<link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><defs><linearGradient id=%22g%22 x1=%220%22 y1=%220%22 x2=%221%22 y2=%221%22><stop offset=%220%25%22 stop-color=%22%231e3a8a%22/><stop offset=%22100%25%22 stop-color=%22%232563eb%22/></linearGradient></defs><rect width=%22100%22 height=%22100%22 rx=%2222%22 fill=%22url(%23g)%22/><circle cx=%2250%22 cy=%2250%22 r=%2230%22 fill=%22%23dbeafe%22/><circle cx=%2250%22 cy=%2250%22 r=%2217%22 fill=%22white%22/><rect x=%2238%22 y=%2238%22 width=%2224%22 height=%2224%22 rx=%224%22 fill=%22%23111827%22/></svg>">
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,Helvetica,sans-serif;background:#eef2f7;color:#0f172a}
.app{max-width:430px;margin:0 auto;min-height:100vh;padding:18px}
.header{display:flex;align-items:center;gap:12px;background:linear-gradient(135deg,#0f172a,#1e3a8a);color:white;padding:18px;border-radius:20px;box-shadow:0 8px 24px rgba(15,23,42,.18)}
.logo{width:44px;height:44px;border-radius:14px;background:linear-gradient(135deg,#1d4ed8,#60a5fa);display:flex;align-items:center;justify-content:center;position:relative;flex:0 0 auto}
.logo:before{content:'';width:24px;height:24px;border-radius:50%;background:#dbeafe;display:block}
.logo:after{content:'';width:12px;height:12px;border-radius:50%;background:white;position:absolute}
.title{font-size:22px;font-weight:700;line-height:1.1}
.subtitle{font-size:12px;color:#dbeafe;margin-top:4px}
.screen{display:none;margin-top:16px}
.screen.active{display:block}
.card{background:white;border:1px solid #dbe4ef;border-radius:20px;padding:16px;box-shadow:0 6px 18px rgba(15,23,42,.05);margin-bottom:14px}
.menu-btn{width:100%;border:none;background:white;border:1px solid #dbe4ef;border-radius:20px;padding:18px;text-align:left;box-shadow:0 6px 18px rgba(15,23,42,.05);margin-bottom:12px;font-size:18px;font-weight:700;color:#0f172a}
.menu-sub{display:block;margin-top:6px;font-size:13px;color:#64748b;font-weight:400}
.row{display:flex;gap:10px}
.row > div{flex:1}
label{display:block;font-size:13px;font-weight:700;color:#334155;margin-bottom:6px}
input,select{width:100%;padding:12px 14px;border-radius:12px;border:1px solid #cbd5e1;font-size:16px;background:white}
input:focus,select:focus{outline:none;border-color:#2563eb}
.quick{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
.quick button{border:none;background:#e2e8f0;color:#334155;padding:9px 12px;border-radius:12px;font-size:13px;font-weight:700}
.quick button.active{background:#2563eb;color:white}
.primary{width:100%;border:none;background:#2563eb;color:white;padding:14px 16px;border-radius:14px;font-size:16px;font-weight:700}
.secondary{width:100%;border:none;background:#0f172a;color:white;padding:14px 16px;border-radius:14px;font-size:16px;font-weight:700}
.back{border:none;background:transparent;color:#475569;font-size:14px;padding:4px 2px 12px}
.result-box{background:#f8fafc;border:1px solid #e2e8f0;border-radius:18px;padding:18px;text-align:center}
.result-label{font-size:13px;color:#64748b;font-weight:700}
.result-value{font-size:34px;font-weight:800;margin-top:4px;letter-spacing:-.02em}
.result-sub{font-size:15px;color:#334155;margin-top:10px}
.quote-head{background:#f8fafc;border:1px solid #e2e8f0;border-radius:16px;padding:14px;line-height:1.7}
.quote-main{font-size:18px;font-weight:700}
.summary-row{display:flex;justify-content:space-between;align-items:center;padding:8px 0;font-size:16px}
.summary-row strong{font-size:20px}
.summary-row.total{border-top:1px solid #e2e8f0;margin-top:6px;padding-top:14px;font-weight:800}
.note{font-size:12px;color:#64748b;line-height:1.5}
.hidden{display:none}
</style>
</head>
<body>
<div class="app">
  <div class="header">
    <div class="logo"></div>
    <div>
      <div class="title">Steel Weight Calculator</div>
      <div class="subtitle">Weight • Quote • Fast field use</div>
    </div>
  </div>  <div id="home" class="screen active">
    <button class="menu-btn" onclick="showScreen('sheet')">판재 중량 계산<span class="menu-sub">실시간 총중량 계산</span></button>
    <button class="menu-btn" onclick="showScreen('quote')">견적 계산<span class="menu-sub">적용단가 · 공급가 · VAT</span></button>
    <div class="card note">V2 테스트 버전입니다. 우선 판재 중량 계산과 견적 계산부터 사용 가능합니다.</div>
  </div>  <div id="sheet" class="screen">
    <button class="back" onclick="showScreen('home')">← 홈으로</button><div class="card">
  <div class="row">
    <div>
      <label>강종</label>
      <input id="grade" value="304" placeholder="예: 304, 430J1L">
    </div>
    <div>
      <label>비중</label>
      <input id="density" value="7.93" oninput="recalcSheet()">
    </div>
  </div>
  <div class="quick">
    <button onclick="setPreset('304',7.93)">304</button>
    <button onclick="setPreset('316L',7.98)">316L</button>
    <button onclick="setPreset('430',7.70)">430</button>
    <button onclick="setPreset('201',7.85)">201</button>
  </div>
</div>

<div class="card">
  <label>표면</label>
  <select id="surface" onchange="toggleSurfaceCustom()">
    <option>2B</option>
    <option>HL</option>
    <option>BA</option>
    <option>NO.1</option>
    <option>PL</option>
    <option>2PL</option>
    <option>SB</option>
    <option>직접입력</option>
  </select>
  <div id="surfaceCustomWrap" class="hidden" style="margin-top:10px">
    <input id="surfaceCustom" placeholder="표면 직접 입력">
  </div>
</div>

<div class="card">
  <label>두께 (mm)</label>
  <select id="thicknessSelect" onchange="toggleThicknessCustom(); recalcSheet()">
    <option>0.3</option><option>0.4</option><option>0.5</option><option>0.6</option><option>0.7</option><option>0.8</option>
    <option>1.0</option><option>1.2</option><option>1.5</option><option>2.0</option><option>2.5</option><option selected>3.0</option>
    <option>4.0</option><option>5.0</option><option>6.0</option><option>7.0</option><option>8.0</option><option>9.0</option>
    <option>10.0</option><option>12.0</option><option>직접입력</option>
  </select>
  <div id="thicknessCustomWrap" class="hidden" style="margin-top:10px">
    <input id="thicknessCustom" placeholder="두께 직접 입력" oninput="recalcSheet()">
  </div>
</div>

<div class="card">
  <label>폭 (mm)</label>
  <input id="width" value="1,219" oninput="formatAndRecalc(this)">
  <div class="quick">
    <button onclick="setValue('width','1,000')">1,000</button>
    <button onclick="setValue('width','1,219')">1,219</button>
    <button onclick="setValue('width','1,524')">1,524</button>
  </div>
</div>

<div class="card">
  <label>길이 (mm)</label>
  <input id="length" value="2,438" oninput="formatAndRecalc(this)">
  <div class="quick">
    <button onclick="setValue('length','2,000')">2,000</button>
    <button onclick="setValue('length','2,438')">2,438</button>
    <button onclick="setValue('length','3,000')">3,000</button>
    <button onclick="setValue('length','3,048')">3,048</button>
  </div>
</div>

<div class="card">
  <label>수량 (장)</label>
  <input id="qty" value="1" oninput="formatAndRecalc(this)">
</div>

<div class="card result-box">
  <div class="result-label">총중량</div>
  <div id="totalWeight" class="result-value">70.7 kg</div>
  <div id="unitWeight" class="result-sub">1장 중량 70.7 kg</div>
</div>

<button class="primary" onclick="showScreen('quote'); syncQuote();">견적 계산</button>

  </div>  <div id="quote" class="screen">
    <button class="back" onclick="showScreen('home')">← 홈으로</button><div class="card quote-head">
  <div id="quoteLine1" class="quote-main">304 2B</div>
  <div id="quoteLine2">3.0 × 1,219 × 2,438</div>
  <div id="quoteLine3">1장</div>
  <div id="quoteLine4" style="font-weight:700;margin-top:6px">총중량 70.7 kg</div>
</div>

<div class="card">
  <label>기준단가</label>
  <input id="basePrice" value="3,500" oninput="formatAndRecalcQuote(this)">
  <label>가공비</label>
  <input id="processPrice" value="200" oninput="formatAndRecalcQuote(this)">
  <label>운송비</label>
  <input id="shippingPrice" value="50" oninput="formatAndRecalcQuote(this)">
  <label>기타</label>
  <input id="etcPrice" value="0" oninput="formatAndRecalcQuote(this)">
  <label>마진</label>
  <input id="marginPrice" value="100" oninput="formatAndRecalcQuote(this)">
</div>

<div class="card">
  <div class="summary-row"><span>적용단가</span><strong id="appliedUnitPrice">3,850</strong></div>
  <div class="summary-row"><span>공급가</span><span id="supplyPrice">272,195</span></div>
  <div class="summary-row"><span>VAT</span><span id="vat">27,219</span></div>
  <div class="summary-row total"><span>합계</span><strong id="grandTotal">299,414</strong></div>
  <button class="secondary" style="margin-top:14px" onclick="copyQuote()">견적 복사</button>
</div>

  </div>
</div><script>
function showScreen(id){
  document.querySelectorAll('.screen').forEach(el=>el.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

function cleanNumber(v){
  return Number(String(v).replace(/,/g,'').trim()) || 0;
}

function formatTyping(v){
  let raw = String(v).replace(/[^0-9.]/g,'');
  if(raw==='') return '';
  let parts = raw.split('.');
  let intPart = parts[0] || '0';
  let formatted = Number(intPart).toLocaleString('ko-KR');
  if(parts.length>1) return formatted + '.' + parts.slice(1).join('');
  return formatted;
}

function round1(v){
  return Math.round(v*10)/10;
}

function fmt1(v){
  return Number(v).toLocaleString('ko-KR',{minimumFractionDigits:1,maximumFractionDigits:1});
}

function fmt0(v){
  return Math.round(v).toLocaleString('ko-KR');
}

function setPreset(grade,density){
  document.getElementById('grade').value = grade;
  document.getElementById('density').value = density;
  recalcSheet();
}

function setValue(id,val){
  document.getElementById(id).value = val;
  recalcSheet();
}

function toggleSurfaceCustom(){
  const isCustom = document.getElementById('surface').value === '직접입력';
  document.getElementById('surfaceCustomWrap').classList.toggle('hidden', !isCustom);
  syncQuote();
}

function toggleThicknessCustom(){
  const isCustom = document.getElementById('thicknessSelect').value === '직접입력';
  document.getElementById('thicknessCustomWrap').classList.toggle('hidden', !isCustom);
}

function getThickness(){
  const mode = document.getElementById('thicknessSelect').value;
  if(mode === '직접입력') return cleanNumber(document.getElementById('thicknessCustom').value);
  return cleanNumber(mode);
}

function formatAndRecalc(input){
  input.value = formatTyping(input.value);
  recalcSheet();
}

function formatAndRecalcQuote(input){
  input.value = formatTyping(input.value);
  recalcQuote();
}

function getSurface(){
  const mode = document.getElementById('surface').value;
  if(mode === '직접입력') return document.getElementById('surfaceCustom').value || '';
  return mode;
}

function recalcSheet(){
  const density = cleanNumber(document.getElementById('density').value);
  const thickness = getThickness();
  const width = cleanNumber(document.getElementById('width').value);
  const length = cleanNumber(document.getElementById('length').value);
  const qty = cleanNumber(document.getElementById('qty').value);

  const unitRaw = (density * thickness * width * length) / 1000000;
  const totalRaw = unitRaw * qty;
  const unit = round1(unitRaw);
  const total = round1(totalRaw);

  document.getElementById('totalWeight').textContent = fmt1(total) + ' kg';
  document.getElementById('unitWeight').textContent = '1장 중량 ' + fmt1(unit) + ' kg';
  syncQuote();
}

function syncQuote(){
  const grade = document.getElementById('grade').value || '';
  const surface = getSurface();
  const thickness = getThickness();
  const width = cleanNumber(document.getElementById('width').value);
  const length = cleanNumber(document.getElementById('length').value);
  const qty = cleanNumber(document.getElementById('qty').value);
  const totalText = document.getElementById('totalWeight').textContent;

  document.getElementById('quoteLine1').textContent = (grade + ' ' + surface).trim();
  document.getElementById('quoteLine2').textContent = thickness.toFixed(1) + ' × ' + fmt0(width) + ' × ' + fmt0(length);
  document.getElementById('quoteLine3').textContent = fmt0(qty) + '장';
  document.getElementById('quoteLine4').textContent = '총중량 ' + totalText;
  recalcQuote();
}

function recalcQuote(){
  const totalWeight = cleanNumber(document.getElementById('totalWeight').textContent);
  const applied = cleanNumber(document.getElementById('basePrice').value)
    + cleanNumber(document.getElementById('processPrice').value)
    + cleanNumber(document.getElementById('shippingPrice').value)
    + cleanNumber(document.getElementById('etcPrice').value)
    + cleanNumber(document.getElementById('marginPrice').value);

  const supply = Math.round(totalWeight * applied);
  const vat = Math.round(supply * 0.1);
  const grand = supply + vat;

  document.getElementById('appliedUnitPrice').textContent = fmt0(applied);
  document.getElementById('supplyPrice').textContent = fmt0(supply);
  document.getElementById('vat').textContent = fmt0(vat);
  document.getElementById('grandTotal').textContent = fmt0(grand);
}

function copyQuote(){
  const text = `${document.getElementById('quoteLine1').textContent}\n${document.getElementById('quoteLine2').textContent}\n${document.getElementById('quoteLine3').textContent}\n${document.getElementById('quoteLine4').textContent}\n\n적용단가 : ${document.getElementById('appliedUnitPrice').textContent}\n공급가 : ${document.getElementById('supplyPrice').textContent}\nVAT : ${document.getElementById('vat').textContent}\n합계 : ${document.getElementById('grandTotal').textContent}`;
  navigator.clipboard.writeText(text);
  alert('견적이 복사되었습니다.');
}

document.getElementById('grade').addEventListener('input', syncQuote);
document.getElementById('density').addEventListener('input', recalcSheet);
document.getElementById('surface').addEventListener('change', syncQuote);
document.getElementById('surfaceCustom').addEventListener('input', syncQuote);

toggleSurfaceCustom();
toggleThicknessCustom();
recalcSheet();
</script></body>
</html>
