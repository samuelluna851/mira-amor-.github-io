<!DOCTYPE html><html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>te amo Perla 💕</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;600;800&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg1:#ffbfd3; /* rosa pastel */
      --bg2:#b9e4ff; /* azul pastel */
      --bg3:#ffd6a5; /* durazno */
      --txt:#ffffff;
      --glow:#ff4fbf;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0}
    body{
      font-family:"Poppins", system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      color:var(--txt);
      overflow:hidden; /* para que los corazones no generen scroll */
      background: radial-gradient(1200px 800px at 10% 10%, var(--bg1), transparent),
                  radial-gradient(1000px 700px at 90% 20%, var(--bg2), transparent),
                  radial-gradient(1200px 800px at 50% 90%, var(--bg3), transparent),
                  #ff9ac5;
      animation:bgShift 18s ease-in-out infinite alternate;
    }
    @keyframes bgShift{
      0%{filter:hue-rotate(0deg)}
      100%{filter:hue-rotate(20deg)}
    }/* Contenedor principal */
.wrap{
  position:relative;z-index:2;
  height:100%;
  display:flex;flex-direction:column;align-items:center;justify-content:center;
  text-align:center;padding:24px;
}

h1{
  font-size: clamp(38px, 8vw, 96px);
  line-height:1.05;margin:0 0 6px 0;
  font-weight:800;letter-spacing:1px;
  text-shadow:
    0 0 10px rgba(255,79,191,.8),
    0 0 30px rgba(255,79,191,.5),
    0 6px 18px rgba(0,0,0,.25);
}

.sub{
  font-size: clamp(14px, 2.5vw, 20px);
  opacity:.9;margin-bottom:22px;
}

/* Botón */
.btn{
  appearance:none;border:0;cursor:pointer;user-select:none;
  padding:14px 20px;border-radius:999px;
  background:rgba(255,255,255,.2);
  backdrop-filter: blur(6px);
  color:white;font-weight:600;font-size:16px;
  box-shadow:0 10px 30px rgba(0,0,0,.15), inset 0 0 0 2px rgba(255,255,255,.35);
  transition: transform .15s ease, box-shadow .2s ease;
}
.btn:hover{transform: translateY(-2px); box-shadow:0 16px 40px rgba(0,0,0,.2), inset 0 0 0 2px rgba(255,255,255,.5)}

/* Cursor corazón al pasar encima del texto */
.love-cursor{cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24"><path fill="%23ff4fbf" d="M12 21s-6.716-4.478-9.184-7.478C.99 11.265 1.44 7.5 4.65 6.3c1.83-.7 3.66.14 4.66 1.7c1-1.56 2.83-2.4 4.66-1.7c3.21 1.2 3.66 4.965 1.834 7.222C18.716 16.522 12 21 12 21z"/></svg>') 16 16, auto}

/* Canvas para corazones flotantes */
#hearts{position:fixed;inset:0;z-index:1;pointer-events:none}

/* Tipo-escritura */
.typewriter {
  display:inline-block; border-right:2px solid rgba(255,255,255,.85);
  white-space:nowrap; overflow:hidden; vertical-align:bottom;
  animation: blink .9s steps(1) infinite;
}
@keyframes blink{50%{border-color:transparent}}

/* Etiquita mini firma */
.tag{position:fixed; bottom:12px; right:12px; font-size:12px; opacity:.65}

  </style>
</head>
<body>
  <canvas id="hearts"></canvas>  <main class="wrap">
    <h1 class="love-cursor" id="title"><span class="typewriter" id="tw"></span></h1>
    <div class="sub">Haz clic o toca para lanzar corazones ✨</div>
    <button class="btn" id="burst">¡Más amor!</button>
  </main>  <div class="tag">Hecho con ♥ para Perla</div>  <script>
    // ====== Texto con efecto de máquina de escribir ======
    const frase = "te amo Perla 💕"; // <- tu frase exacta
    const tw = document.getElementById('tw');
    let i = 0;
    function type(){
      if(i <= frase.length){
        tw.textContent = frase.substring(0, i);
        i++;
        setTimeout(type, 90);
      } else {
        // quita el cursor tras terminar para un look más limpio
        tw.classList.remove('typewriter');
      }
    }
    type();

    // ====== Corazones flotantes en canvas ======
    const canvas = document.getElementById('hearts');
    const ctx = canvas.getContext('2d');
    let W, H, DPR;

    function resize(){
      DPR = Math.min(window.devicePixelRatio || 1, 2);
      W = canvas.width = Math.floor(innerWidth * DPR);
      H = canvas.height = Math.floor(innerHeight * DPR);
      canvas.style.width = innerWidth + 'px';
      canvas.style.height = innerHeight + 'px';
    }
    addEventListener('resize', resize);
    resize();

    // Utilidad: dibujar un corazón en (x,y) con tamaño s
    function drawHeart(x, y, s, a){
      ctx.save();
      ctx.translate(x, y);
      ctx.scale(s, s);
      ctx.globalAlpha = a;
      ctx.beginPath();
      // Curva de corazón con Beziers
      ctx.moveTo(0, -10);
      ctx.bezierCurveTo(12, -24, 36, -8, 0, 22);
      ctx.bezierCurveTo(-36, -8, -12, -24, 0, -10);
      const grad = ctx.createRadialGradient(0,0,2, 0,0,26);
      grad.addColorStop(0, 'rgba(255,255,255,.95)');
      grad.addColorStop(.2, 'rgba(255,160,210,.95)');
      grad.addColorStop(1, 'rgba(255,79,191,.85)');
      ctx.fillStyle = grad;
      ctx.fill();
      ctx.restore();
    }

    const hearts = [];
    function spawn(x, y, amount=8, spread=80){
      for(let i=0;i<amount;i++){
        const angle = (Math.random()*Math.PI*2);
        const speed = 0.5 + Math.random()*1.8;
        hearts.push({
          x: (x||Math.random()*W),
          y: (y||H + 40),
          vx: Math.cos(angle)*speed*0.6,
          vy: -Math.abs(Math.sin(angle))*speed - (0.3+Math.random()*0.7),
          size: (18 + Math.random()*26) * (DPR*0.8),
          life: 1,
          rot: Math.random()*Math.PI,
        });
      }
    }

    // lluvia suave constante
    setInterval(()=> spawn(undefined, undefined, 3), 350);

    // Explosión de corazones al hacer clic/toque
    function burstAt(px, py){
      const x = px * (window.devicePixelRatio||1);
      const y = py * (window.devicePixelRatio||1);
      spawn(x, y, 24);
    }
    addEventListener('pointerdown', e => burstAt(e.clientX, e.clientY));
    document.getElementById('burst').addEventListener('click', () => {
      const rect = document.getElementById('title').getBoundingClientRect();
      burstAt(rect.left + rect.width/2, rect.top + rect.height/2);
    });

    // animación
    function tick(){
      ctx.clearRect(0,0,W,H);
      for(let i=hearts.length-1;i>=0;i--){
        const h = hearts[i];
        h.x += h.vx;
        h.y += h.vy;
        h.vy += 0.01; // "gravedad" suave hacia arriba negativa ya aplicada
        h.life -= 0.005 + Math.random()*0.004;
        drawHeart(h.x, h.y, h.size/26, Math.max(h.life, 0));
        // eliminar cuando sale de pantalla o vida se acaba
        if(h.y < -60 || h.life <= 0){ hearts.splice(i,1); }
      }
      requestAnimationFrame(tick);
    }
    tick();
  </script></body>
</html>
