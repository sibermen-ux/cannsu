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

    .lock-card{
      width:100%;max-width:700px;
      background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));
      border-radius:16px;padding:48px;
      box-shadow:0 10px 30px rgba(2,6,23,0.7);backdrop-filter:blur(6px);
    }

    .logo{display:flex;gap:14px;align-items:center}
    .logo .dot{width:14px;height:14px;background:var(--accent);border-radius:50%}
    h1{margin:18px 0 6px;font-weight:800;letter-spacing:0.6px}
    p.lead{margin:0 0 24px;color:#cfd8ff66}
    .pwd-row{display:flex;gap:12px}
    input[type=password]{
      flex:1;padding:14px;border-radius:10px;border:1px solid rgba(255,255,255,0.06);
      background:transparent;color:#fff;font-size:16px
    }
    button.btn{
      padding:12px 18px;border-radius:10px;border:none;
      background:linear-gradient(90deg,var(--accent),#ff7ab8);
      font-weight:700;cursor:pointer
    }

    .stage-two{
      display:none;min-height:100vh;padding:40px;
      background:linear-gradient(180deg,#050712 0%,rgba(0,0,0,0.3) 60%);
    }

    .poem{
      font-size:22px;max-width:700px;
      line-height:1.8;font-weight:600;
      color:#dbe7ffcc;text-align:center;margin:0 auto;
    }

    canvas#confetti{
      position:fixed;left:0;top:0;width:100%;height:100%;
      pointer-events:none;z-index:40;
    }

    .falling-area{
      position:fixed;top:0;left:0;
      width:100%;height:100%;
      pointer-events:none;overflow:hidden;
    }
    .emoji{
      position:absolute;top:-50px;font-size:30px;
      animation:fall linear infinite;
    }
    @keyframes fall{
      0%{transform:translateY(-60px) rotate(0deg);opacity:1;}
      100%{transform:translateY(110vh) rotate(360deg);opacity:0;}
    }
  </style>
</head>


  <!-- Şifre Ekranı -->
  <main class="center" id="phase1">
    <div class="lock-card">
      <div class="logo"><div class="dot"></div><div style="font-weight:700">Hoş geldin</div></div>
      <h1>Giriş</h1>
      <p class="lead">Erişim için lütfen şifreyi girin.</p>

      <div class="pwd-row">
        <input id="pwdInput" type="password" placeholder="Şifre" />
        <button id="unlockBtn" class="btn">Gir</button>
      </div>
    </div>
  </main>

  <!-- Hediye Ekranı -->
  <section id="phase2" class="stage-two">
    <canvas id="confetti"></canvas>
    <div class="falling-area"></div>

    <div class="poem">
      Gönderiyorum rüzgarla en güzel duamı —<br>
      Kalbim seninle attıkça, mesafeler hafifler sevgilim.<br>
      Gece çöker, yıldızlar parlar; her biri sana anlattığım bir hikâye…<br>
      Uzakta olsan da, içimde açan en sıcak baharsın.<br>
      Seni düşündükçe yol olur bana tüm karanlıklar,<br>
      Ve ben, en çok sana varırken tamamlanırım.<br><br>
      Bil ki: Mesafe sadece rakamdır,<br>
      Kalbin yolunu biliyorsa — geri kalan her şey yakınlaşır.
    </div>
  </section>

<script>
  const PASSWORD = "1203";

  const phase1 = document.getElementById("phase1");
  const phase2 = document.getElementById("phase2");
  const pwdInput = document.getElementById("pwdInput");
  const unlockBtn = document.getElementById("unlockBtn");

  function showPhase2(){
    phase1.style.display = "none";
    phase2.style.display = "block";
    startConfetti();
  }

  unlockBtn.addEventListener("click", ()=>{
    if(pwdInput.value.trim() === PASSWORD){
      showPhase2();
    } else {
      pwdInput.value = "";
      pwdInput.placeholder = "Hatalı şifre!";
    }
  });

  pwdInput.addEventListener("keydown", e=>{
    if(e.key==="Enter") unlockBtn.click();
  });

  function startConfetti(duration=2000){
    const canvas = document.getElementById("confetti");
    canvas.width = innerWidth; canvas.height = innerHeight;
    const ctx = canvas.getContext("2d");

    const colors = ['#ff5ca8','#ffd166','#8ecae6','#bdb2ff','#ff7ab8'];
    const pieces = [];

    for(let i=0;i<120;i++){
      pieces.push({
        x:Math.random()*canvas.width,
        y:-Math.random()*canvas.height,
        vy:2+Math.random()*6,
        angle:Math.random()*360,
        size:6+Math.random()*8,
        color:colors[Math.floor(Math.random()*colors.length)]
      });
    }

    let t0 = performance.now();
    function frame(t){
      ctx.clearRect(0,0,canvas.width,canvas.height);
      for(const p of pieces){
        p.y += p.vy;
        p.x += Math.sin((t+p.angle)/200)*2;
        p.angle += 4;
        ctx.save();
        ctx.translate(p.x,p.y);
        ctx.rotate(p.angle*0.017);
        ctx.fillStyle=p.color;
        ctx.fillRect(-p.size/2,-p.size/2,p.size,p.size);
        ctx.restore();
      }
      if(t - t0 < duration) requestAnimationFrame(frame);
    }
    requestAnimationFrame(frame);
  }

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



