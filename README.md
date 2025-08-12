<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Chatbot Gốm Mỹ Nghệ Biên Hòa</title>
<meta name="description" content="Chatbot giao lưu & tra cứu về bảo tồn nghệ thuật làm gốm mỹ nghệ Biên Hòa" />
<link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@500;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#f8f4f0; --panel:#ffffff; --muted:#6d6d6d; --text:#2e2e2e;
  --accent:#c6a664; /* gold */ --accent-2:#2e8e7e; /* jade */
  --bot:#f5efe7; --user:#ece5db; --ring:rgba(198,166,100,.35);
}
[data-theme="dark"]{
  --bg:#0b0b0c; --panel:#121214; --muted:#a7a29a; --text:#ece8df;
  --accent:#c6a664; --accent-2:#3fbda8; --bot:#1a1a1d; --user:#172126; --ring:rgba(198,166,100,.35);
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}
body{font-family:Inter, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, 'Apple Color Emoji','Segoe UI Emoji';background:var(--bg);color:var(--text);line-height:1.6}
.app{max-width:1080px;margin:0 auto;padding:16px;display:grid;grid-template-rows:auto 1fr}
header{background:linear-gradient(135deg, rgba(255,255,255,.6), rgba(255,255,255,.2));border:1px solid rgba(0,0,0,.06);border-radius:16px;padding:16px 18px;display:flex;gap:12px;align-items:center;box-shadow:0 12px 28px rgba(0,0,0,.06)}
header h1{font-family:'Playfair Display', serif;font-size:24px;color:var(--accent)}
.sub{color:var(--muted);font-size:13px}
.header-actions{margin-left:auto;display:flex;gap:8px}
main{display:grid;grid-template-columns:2fr 1fr;gap:16px;margin-top:16px}
.wrap,.panel{background:linear-gradient(180deg, var(--panel), rgba(255,255,255,0));border-radius:16px;padding:16px;border:1px solid rgba(0,0,0,.06);box-shadow:0 8px 24px rgba(0,0,0,.06)}
#scroll{max-height:60vh;overflow:auto;padding-right:4px}
.msg{display:grid;grid-template-columns:auto 1fr;gap:10px;margin:10px 0;align-items:flex-start}
.msg .avatar{width:34px;height:34px;border-radius:10px;display:grid;place-items:center;background:var(--bot)}
.msg.user .avatar{background:var(--user)}
.bubble{background:#fffdf8;border:1px solid rgba(0,0,0,.06);border-radius:14px;padding:10px 14px;box-shadow:0 2px 8px rgba(0,0,0,.06)}
.msg.user .bubble{background:var(--user)}
.msg.bot .bubble{background:var(--bot)}
.time{color:var(--muted);font-size:11px;margin-top:6px}
.tools{display:flex;gap:8px;margin-top:8px}
.tool{font-size:11px;padding:6px 8px;border-radius:999px;border:1px solid rgba(0,0,0,.08);background:transparent;color:inherit;cursor:pointer}
.tool:hover{border-color:var(--accent-2)}
.input{display:flex;gap:8px;margin-top:10px}
.text{flex:1;padding:12px 14px;border-radius:12px;border:1px solid rgba(0,0,0,.12);background:#fff;font-size:14px;box-shadow:0 0 0 0 var(--ring)}
.text:focus{border-color:var(--accent);outline:none;box-shadow:0 0 0 4px var(--ring)}
.btn{appearance:none;border:1px solid rgba(0,0,0,.12);background:linear-gradient(180deg, rgba(255,255,255,.08), rgba(0,0,0,.02));color:var(--text);padding:0 14px;border-radius:12px;font-weight:600;cursor:pointer;height:44px;transition:transform .04s ease, box-shadow .2s ease}
.btn:hover{border-color:var(--accent-2);box-shadow:0 8px 20px rgba(0,0,0,.12)}
.btn:active{transform:translateY(1px)}
.suggest{display:flex;flex-wrap:wrap;gap:8px;margin-top:8px}
.chip{background:transparent;border:1px solid rgba(0,0,0,.12);padding:8px 12px;border-radius:999px;font-size:12px;cursor:pointer;color:inherit}
.chip:hover{border-color:var(--accent-2)}
.small{font-size:12px;color:var(--muted)}
@media (max-width: 860px){
  main{grid-template-columns:1fr}
  #scroll{max-height:50vh}
}
</style>
</head>
<body>
<div class="app">
  <header>
    <div>
      <h1>Gốm Mỹ Nghệ Biên Hòa — Chatbot</h1>
      <div class="sub">Hỏi đáp về lịch sử, kỹ thuật, mẫu mã & bảo tồn gốm Biên Hòa</div>
    </div>
    <div class="header-actions">
      <button class="btn" id="theme">Chủ đề</button>
    </div>
  </header>
  <main>
    <section class="wrap">
      <div id="scroll"></div>
      <div class="suggest" id="suggest"></div>
      <div class="input">
        <textarea id="input" class="text" placeholder="Nhập câu hỏi..."></textarea>
        <button class="btn" id="send">Gửi</button>
      </div>
    </section>
    <aside class="panel">
      <h3 style="font-family:'Playfair Display', serif;color:var(--accent);margin-bottom:8px">Tiện ích</h3>
      <p class="small">• Bộ sưu tập hình ảnh gốm • Video phỏng vấn nghệ nhân • Lịch hội chợ & workshop</p>
      <div style="display:flex;gap:8px;margin-top:10px;flex-wrap:wrap">
        <button class="btn" id="openGallery">Bộ sưu tập</button>
        <button class="btn" id="exportChat">Xuất hội thoại</button>
      </div>
      <hr style="margin:14px 0;border:0;border-top:1px solid rgba(0,0,0,.08)">
      <p class="small">Mẹo: Nhấn vào gợi ý dưới khung chat để hỏi nhanh. Dùng Shift+Enter để xuống dòng.</p>
    </aside>
  </main>
</div>
<dialog id="gallery" style="max-width:900px;width:clamp(320px,90vw,900px);border:1px solid rgba(0,0,0,.12);border-radius:16px;padding:0;background:var(--panel);color:var(--text)">
  <div style="display:flex;align-items:center;gap:8px;padding:12px 14px;border-bottom:1px solid rgba(0,0,0,.08)">
    <strong style="font-family:'Playfair Display',serif">Bộ sưu tập (demo)</strong>
    <div style="margin-left:auto"></div>
    <button class="btn" id="closeGallery" style="height:auto;padding:6px 10px">Đóng</button>
  </div>
  <div style="padding:14px">
    <p class="small">Bạn có thể thay phần này bằng hình ảnh gốm Biên Hòa thực tế.</p>
    <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(160px,1fr));gap:10px">
      <div style="aspect-ratio:1/1;background:linear-gradient(135deg,#d3c3a5,#efe6d8);border-radius:12px;display:grid;place-items:center">Ảnh 1</div>
      <div style="aspect-ratio:1/1;background:linear-gradient(135deg,#bcd6ce,#e6f2ee);border-radius:12px;display:grid;place-items:center">Ảnh 2</div>
      <div style="aspect-ratio:1/1;background:linear-gradient(135deg,#e7d7c2,#fff4e5);border-radius:12px;display:grid;place-items:center">Ảnh 3</div>
      <div style="aspect-ratio:1/1;background:linear-gradient(135deg,#d9cbb4,#f5ede4);border-radius:12px;display:grid;place-items:center">Ảnh 4</div>
    </div>
  </div>
</dialog>
<script>
const el = s=>document.querySelector(s);
const scroll = el('#scroll');
const input = el('#input');
const sendBtn = el('#send');
const suggest = el('#suggest');
const themeBtn = el('#theme');
const exportBtn = el('#exportChat');
const openGalleryBtn = el('#openGallery');
const gallery = el('#gallery');
const closeGalleryBtn = el('#closeGallery');

const THEME_KEY='gom-bien-hoa-theme';
const transcript=[];

function applyTheme(t){
  document.documentElement.setAttribute('data-theme', t==='dark'?'dark':'');
  localStorage.setItem(THEME_KEY, t);
}
function sanitize(str){return (str||'').replace(/[<>]/g, s=>({'<':'&lt;','>':'&gt;'}[s]));}
function now(){const d=new Date();return d.toLocaleString('vi-VN',{hour:'2-digit',minute:'2-digit'});}

function addMessage(role, text){
  const wrap=document.createElement('div');
  wrap.className='msg '+role;
  const safe=sanitize(text);
  const tools = role==='bot' ? `<div class="tools">
      <button class="tool" data-copy>📋 Sao chép</button>
      <button class="tool" data-like>👍 Hữu ích</button>
      <button class="tool" data-dislike>👎 Chưa rõ</button>
    </div>` : '';
  wrap.innerHTML=`
    <div class="avatar">${role==='bot'?'🤖':'🧑'}</div>
    <div>
      <div class="bubble">${safe}</div>
      ${tools}
      <div class="time">${now()}</div>
    </div>`;
  scroll.appendChild(wrap);
  scroll.scrollTop=scroll.scrollHeight;
  transcript.push({role,text});
}

function setSuggestions(list){
  suggest.innerHTML='';
  list.forEach(t=>{
    const b=document.createElement('button');
    b.className='chip'; b.textContent=t; b.onclick=()=>{input.value=t; handleSend();};
    suggest.appendChild(b);
  });
}

function handleSend(){
  const t=input.value.trim(); if(!t) return;
  addMessage('user', t);
  input.value='';
  // Demo trả lời: bạn có thể thay bằng logic Q&A hoặc API sau này
  const demo = {
    'gốm biên hòa là gì':'Gốm Biên Hòa là dòng gốm mỹ nghệ tại Đồng Nai, nổi bật bởi tạo hình thủ công và gam men trầm (mô tả demo).',
    'men đặc trưng':'Các tông xanh rêu, nâu hổ phách, đôi lúc phối men tro/rạn để nhấn mạnh chất mộc (demo).',
    'bảo tồn':'Bạn có thể tham gia workshop, ghi chép quy trình, số hóa hoa văn và ủng hộ mua trực tiếp từ nghệ nhân (demo).'
  };
  const key = t.toLowerCase();
  const ans = Object.keys(demo).find(k=> key.includes(k))
    ? demo[Object.keys(demo).find(k=> key.includes(k))]
    : 'Mình là bản demo — hãy cho mình biết thêm câu hỏi hoặc bấm "Bộ sưu tập" để xem ảnh minh họa.';
  addMessage('bot', ans+"

<small class='small'>#gốm #biên_hòa #bảo_tồn</small>");
}

// Delegation for tools
scroll.addEventListener('click', e=>{
  const t=e.target;
  if(t.matches('[data-copy]')){
    const bubble=t.closest('.msg').querySelector('.bubble');
    navigator.clipboard.writeText(bubble.textContent.trim());
    t.textContent='Đã sao chép'; setTimeout(()=>t.textContent='📋 Sao chép',1200);
  }
  if(t.matches('[data-like]')) t.textContent='👍 Cảm ơn!';
  if(t.matches('[data-dislike]')) t.textContent='👎 Đã ghi nhận';
});

function exportChat(){
  const lines = transcript.map(m=>`[${m.role}] ${m.text.replace(/
/g,' ')}
`).join('');
  const blob = new Blob([lines], {type:'text/plain'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href=url; a.download='hoi-thoai-gom-bien-hoa.txt'; a.click();
  URL.revokeObjectURL(url);
}

// Events
sendBtn.onclick=handleSend;
input.addEventListener('keydown',e=>{ if(e.key==='Enter' && !e.shiftKey){ e.preventDefault(); handleSend(); } });

const savedTheme = localStorage.getItem(THEME_KEY)||'light';
applyTheme(savedTheme);
themeBtn.onclick=()=>{ const cur=document.documentElement.getAttribute('data-theme')==='dark'?'light':'dark'; applyTheme(cur); };
exportBtn.onclick=exportChat;
openGalleryBtn.onclick=()=> gallery.showModal();
closeGalleryBtn.onclick=()=> gallery.close();

// Init
addMessage('bot','Xin chào! Mình là chatbot về <strong>Gốm Mỹ Nghệ Biên Hòa</strong>. Hãy hỏi mình về lịch sử, kỹ thuật men, hoa văn, sản phẩm tiêu biểu hoặc cách đóng góp bảo tồn.');
setSuggestions([
  'Gốm Biên Hòa là gì?',
  'Men đặc trưng của gốm Biên Hòa?',
  'Những cách góp phần bảo tồn?'
]);
</script>
</body>
</html>
