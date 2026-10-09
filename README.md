<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>【遊戲名稱】</title>
<style>
*{box-sizing:border-box;font-family:system-ui, sans-serif;margin:0;padding:0;}
body{background:#f4f7fa;color:#222;line-height:1.6;padding:16px;max-width:900px;margin:0 auto;}
header{text-align:center;margin-bottom:24px;}
h1{color:#334155;font-size:26px;margin-bottom:6px;}
.desc{color:#64748b;margin-bottom:16px;}
.box{background:#fff;padding:24px;border-radius:16px;box-shadow:0 4px 12px #00000010;margin-bottom:20px;}
button{width:100%;padding:14px;background:#2563eb;color:#fff;border:none;border-radius:10px;font-size:16px;cursor:pointer;margin-top:10px;}
button:disabled{background:#94a3b8;cursor:not-allowed;}
.secondary-btn{background:#64748b;margin-top:8px;}
.share-link-area{margin-top:16px;}
#shareUrl{width:100%;padding:10px;border:1px solid #cbd5e1;border-radius:8px;margin:8px 0;}
.copy-tip{color:#22c55e;font-size:14px;display:none;}
.result-area{margin:20px 0;min-height:120px;padding:16px;background:#f8fafc;border-radius:10px;}

@media print {
  body {background:#fff;}
  button, .share-link-area {display:none !important;}
  .box {box-shadow:none; padding:0;}
}
</style>
</head>
<body>
<header>
<h1>【遊戲名稱】</h1>
<p class="desc">簡單互動小遊戲｜結果可列印與分享</p>
</header>

<div class="box">
  <div id="gameArea">
    <!-- 遊戲主畫面 -->
  </div>
  <div class="result-area" id="resultBox" style="display:none;">
    <h3>你的結果</h3>
    <div id="resultText"></div>
  </div>

  <button id="printBtn">🖨️ 列印報告 / 另存PDF</button>
  <div class="share-link-area">
    <p>🔗 分享結果連結</p>
    <input id="shareUrl" readonly>
    <button id="copyLinkBtn" class="secondary-btn">複製連結</button>
    <div class="copy-tip" id="copyTip">✅ 已複製到剪貼簿</div>
  </div>
  <button id="restartBtn">重新開始</button>
</div>

<script>
const printBtn = document.getElementById('printBtn');
const shareUrlInput = document.getElementById('shareUrl');
const copyLinkBtn = document.getElementById('copyLinkBtn');
const copyTip = document.getElementById('copyTip');
const restartBtn = document.getElementById('restartBtn');
const resultBox = document.getElementById('resultBox');
const resultText = document.getElementById('resultText');

let currentResult = null;

// 列印
printBtn.onclick = ()=> window.print();
// 複製連結
copyLinkBtn.onclick = async ()=>{
  await navigator.clipboard.writeText(shareUrlInput.value);
  copyTip.style.display = "block";
  setTimeout(()=>copyTip.style.display="none",2000);
};

// 產生分享URL
function buildShareUrl(res){
  const payload = btoa(JSON.stringify(res));
  const url = `${location.origin}${location.pathname}?r=${payload}`;
  shareUrlInput.value = url;
}

// 從URL載入分享結果
function loadFromUrlParam(){
  const params = new URLSearchParams(location.search);
  const dataStr = params.get("r");
  if(!dataStr) return;
  try{
    const json = JSON.parse(atob(dataStr));
    renderResult(json);
  }catch(e){
    console.log("無效連結");
  }
}

// 渲染結果
function renderResult(res){
  currentResult = res;
  resultBox.style.display="block";
  resultText.innerHTML = res.html;
  buildShareUrl(res);
}

// ======================
// 這裡替換成每個遊戲專屬JS
// ======================
function runGame(){
  // 遊戲邏輯，算出結果後呼叫 renderResult({html:"結果內容"})
}

restartBtn.onclick = ()=>{
  resultBox.style.display="none";
  history.replaceState({}, document.title, location.pathname);
};

window.onload = loadFromUrlParam;
</script>
</body>
</html>
