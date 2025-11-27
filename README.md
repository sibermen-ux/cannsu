<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Gizli Hediye</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;600;800&display=swap" rel="stylesheet">
  <style>
    :root{--bg:#0b0f1a;--card:#0f1724;--accent:#ff5ca8;--glass:rgba(255,255,255,0.06)}
    *{box-sizing:border-box;font-family:Montserrat,system-ui,-apple-system,'Segoe UI',Roboto,Arial}
    html,body{height:100%;margin:0;background:linear-gradient(160deg,#071025 0%, #081229 50%, #0b0f1a 100%);color:#fff}
    .center{display:flex;align-items:center;justify-content:center;height:100vh;padding:20px}

    /* 1. AŞAMA: Şifre ekranı */
    .lock-card{width:100%;max-width:700px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:16px;padding:48px;box-shadow:0 10px 30px rgba(2,6,23,0.7);backdrop-filter:blur(6px);}    
    .logo{display:flex;gap:14px;align-items:center}
    .logo .dot{width:14px;height:14px;background:var(--accent);border-radius:50%}
    h1{margin:18px 0 6px;font-weight:800;letter-spacing:0.6px}
    p.lead{margin:0 0 24px;color:#cfd8ff66}
    .pwd-row{display:flex;gap:12px}
    input[type=password]{flex:1;padding:14px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:#fff;font-size:16px}
    button.btn{padding:12px 18px;border-radius:10px;border:none;background:linear-gradient(90deg,var(--accent),#ff7ab8);font-weight:700;cursor:pointer}
    .note{font-size:13px;color:#c7d2fe66;margin-top:12px}

    /* 2. AŞAMA: Hediye ekranı */
    .stage-two{display:none;min-height:100vh;padding:40px;background:linear-gradient(180deg,#050712 0%, rgba(0,0,0,0.3) 60%)}
    .hero{display:grid;grid-template-columns:1fr 420px;gap:30px;align-items:center;max-width:1200px;margin:0 auto}
    .gift-card{background:var(--card);padding:28px;border-radius:16px;box-shadow:0 10px 40px rgba(2,6,23,0.6)}
    .gift-title{font-size:28px;font-weight:800;margin-bottom:12px}
    .poem{color:#dbe7ffcc;line-height:1.6}
    .present{border-radius:14px;overflow:hidden;background:linear-gradient(180deg,#111827,#0b1220);display:flex;flex-direction:column;align-items:center;padding:20px}
    .present img{width:100%;max-width:360px;border-radius:12px}
    .play-music{margin-top:12px;padding:10px 14px;border-radius:10px;background:var(--glass);border:1px solid rgba(255,255,255,0.04);cursor:pointer}
    .sparkles{position:absolute;pointer-events:none;mix-blend-mode:screen}

    /* confetti canvas */
    canvas#confetti{position:fixed;left:0;top:0;width:100%;height:100%;z-index:40;pointer-events:none}

    footer{position:fixed;left:12px;bottom:12px;font-size:12px;color:#99a4ff66}

    @media (max-width:900px){.hero{grid-template-columns:1fr;}.gift-card{order:2}}
  
    /* Parıldayan Kalp Animasyonu */
    .pulse-heart{
      width:140px;
      height:140px;
      background:red;
      position:relative;
      transform:rotate(45deg);
      animation:pulse 1.8s infinite ease-in-out;
      box-shadow:0 0 30px rgba(255,80,120,0.65);
    }
    .pulse-heart:before,
    .pulse-heart:after{
      content:"";
      width:140px;
      height:140px;
      background:red;
      border-radius:50%;
      position:absolute;
    }
    .pulse-heart:before{top:-70px;left:0;}
    .pulse-heart:after{left:-70px;top:0;}

    @keyframes pulse{
      0%,100%{transform:rotate(45deg) scale(1); box-shadow:0 0 26px rgba(255,80,120,0.6);} 
      50%{transform:rotate(45deg) scale(1.18); box-shadow:0 0 38px rgba(255,80,140,0.95);} 
    }


    /* Sürekli düşen kalpler ve emojiler */
    .falling-area{
      position:fixed;
      top:0;left:0;
      width:100%;height:100%;
      pointer-events:none;
      overflow:hidden;
    }
    .emoji{
      position:absolute;
      top:-50px;
      font-size:30px;
      animation:fall linear infinite;
    }
    @keyframes fall{
      0%{transform:translateY(-60px) rotate(0deg);opacity:1;}
      100%{transform:translateY(110vh) rotate(360deg);opacity:0;}
    }
</style>
</head>
<body>
  <!-- 1. AŞAMA: Hiçbir ipucu yok. Sadece şifre ekranı. -->
  <main class="center" id="phase1">
    <div class="lock-card" aria-live="polite">
      <div class="logo"><div class="dot"></div><div style="font-weight:700">Hoş geldin</div></div>
      <h1>Giriş</h1>
      <p class="lead">Erişim için lütfen şifreyi girin.</p>
      <div class="pwd-row">
        <input id="pwdInput" type="password" placeholder="Şifre" aria-label="şifre gir" />
        <button id="unlockBtn" class="btn">Gir</button>
      </div>
      <div class="note"></div>
    </div>
  </main>

  <!-- 2. AŞAMA: Uzak mesafedeki sevgiliye özel gösterişli hediye -->
  <section id="phase2" class="stage-two">
    <canvas id="confetti"></canvas>

    <!-- PARILDAYAN KALP -->
    <div class="falling-area"></div>
    </div>
    <canvas id="confetti"></canvas>
    <div style="max-width:1200px;margin:30px auto 0;position:relative;">
      <div class="hero" style="display:flex;flex-direction:column;align-items:center;text-align:center;gap:40px;">
  <div class="poem" style="font-size:22px;max-width:700px;line-height:1.8;font-weight:600;">
    Gönderiyorum rüzgarla en güzel duamı —<br>
    Kalbim seninle attıkça, mesafeler hafifler sevgilim.<br>
    Gece çöker, yıldızlar parlar; her biri sana anlattığım bir hikâye…<br>
    Uzakta olsan da, içimde açan en sıcak baharsın.<br>
    Seni düşündükçe yol olur bana tüm karanlıklar,<br>
    Ve ben, en çok sana varırken tamamlanırım.<br><br>
    Bil ki: Mesafe sadece rakamdır,<br>
    Kalbin yolunu biliyorsa — geri kalan her şey yakınlaşır.
  </div>
</div>
              <div style="color:#bfc9ff99"></div>
            </div>
            <div style="text-align:right;color:#9aa6ff99;font-size:13px"</div>
          </div>

          <div style="margin-top:18px;display:flex;gap:16px;flex-direction:column">
            <div class="poem">
              
           </div>

            <div style="display:flex;gap:12px;align-items:center;margin-top:8px">
              
          </div>
          </div>
        </div>

        
      </div>
    </div>
   
  </section>

  <audio id="bgMusic" loop>
    <!-- Örnek: kullanıcı kendi mp3'ünü koysun. Tarayıcılar otomatik çalmayı engelleyebilir; 'Müziği Çal' butonuna basınca oynatılır. -->
    <source src="" type="audio/mpeg">
  </audio>

  <script>
    // === KOLAY ŞİFRE (DEĞİŞTİRİN) ===
    // Buradaki 'PASSWORD' değişkenini kendinize özel yapın.
    const PASSWORD = "1203"; // <-- bu değeri mutlaka değiştirin

    const phase1 = document.getElementById('phase1');
    const phase2 = document.getElementById('phase2');
    const pwdInput = document.getElementById('pwdInput');
    const unlockBtn = document.getElementById('unlockBtn');

    function showPhase2(){
      phase1.style.display='none';
      phase2.style.display='block';
      startConfetti();
    }

    unlockBtn.addEventListener('click', ()=>{
      const v = pwdInput.value.trim();
      if(v===PASSWORD){
        showPhase2();
      } else {
        pwdInput.value='';
        pwdInput.placeholder='Hatalı şifre';
        pwdInput.focus();
      }
    });

    pwdInput.addEventListener('keydown', (e)=>{ if(e.key==='Enter') unlockBtn.click(); });

    // === MÜZİK OYNATMA ===
    const bg = document.getElementById('bgMusic');
    const playBtn = document.getElementById('playBtn');
    playBtn.addEventListener('click', ()=>{
      // Kullanıcı kendi mp3 URL'sini <audio> etiketine ekleyin. Veya tarayıcı izin verirse oynatılır.
      // DİKKAT: Buradaki mesaj çift tırnak içinde yazıldı, böylece Türkçe'deki kesme işareti (') string'i bozmaz.
      if(!bg.src || bg.src === ''){
        alert("Müziği kullanmak için index.html içinde <audio> kaynağını (src) kendi mp3 URL'nizle değiştirin.");
        return;
      }
      bg.play().catch(()=>{alert('Tarayıcı otomatik çalmayı engelledi. Lütfen izin verin.');});
    });

    // === KART İNDİRME (basit) ===
    document.getElementById('downloadBtn').addEventListener('click', ()=>{
      // Burada hediye kartının anlık görünümünü PNG'ye dönüştürebiliriz.
      // Basit bir yaklaşım: hediye görselini indir
      const img = document.getElementById('giftImg');
      const link = document.createElement('a');
      link.href = img.src;
      link.download = 'hediye-kutusu.jpg';
      link.click();
    });

    // === SÜRPRİZ ANİMASYON ===
    document.getElementById('surpriseBtn').addEventListener('click', ()=>{
      startConfetti(800);
    });

    // === KÜÇÜK CONFETTI KÜTÜPHANESİ ===
    function startConfetti(duration=2000){
      const canvas = document.getElementById('confetti');
      canvas.width = innerWidth; canvas.height = innerHeight;
      const ctx = canvas.getContext('2d');
      const colors = ['#ff5ca8','#ffd166','#8ecae6','#bdb2ff','#ff7ab8'];
      const pieces = [];
      for(let i=0;i<120;i++){
        pieces.push({x:Math.random()*canvas.width,y:-Math.random()*canvas.height,vy:2+Math.random()*6,angle:Math.random()*360,size:6+Math.random()*8,color:colors[Math.floor(Math.random()*colors.length)]});
      }
      let t0 = performance.now();
      function frame(t){
        ctx.clearRect(0,0,canvas.width,canvas.height);
        for(const p of pieces){
          p.y += p.vy; p.x += Math.sin((t+p.angle)/200)*2; p.angle += 4;
          ctx.save(); ctx.translate(p.x,p.y); ctx.rotate(p.angle*0.017); ctx.fillStyle=p.color; ctx.fillRect(-p.size/2,-p.size/2,p.size,p.size); ctx.restore();
        }
        if(t - t0 < duration) requestAnimationFrame(frame); else ctx.clearRect(0,0,canvas.width,canvas.height);
      }
      requestAnimationFrame(frame);
    }

    // Prevent showing anything about phase2 on the phase1 HTML (no hints).
    // Tüm ikinci aşama içeriği sadece doğru şifre girildiğinde gösterilir.

    // Eğer isterseniz: bu dosyayı sunucuya koyup index.html'i ana sayfa yapabilirsiniz.
  </script>
<script>
  // Sürekli düşen kalpler ve emojiler
  const emojis = ['❤️','💖','✨','💘','💞','💕','🌙','⭐'];
  const area = document.querySelector('.falling-area');

  function drop(){
    const e = document.createElement('div');
    e.className='emoji';
    e.innerText = emojis[Math.floor(Math.random()*emojis.length)];
    e.style.left = Math.random()*100 + 'vw';
    e.style.animationDuration = (3 + Math.random()*4)+'s';
    area.appendChild(e);
    setTimeout(()=>e.remove(),6000);
  }
  setInterval(drop, 350);
</script>
html></body>
</

