<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Una función especial · para Adriana</title>

<!-- Tipografías: Fraunces (serif cálido y nostálgico) + Nunito (redondeada y amable).
     Si se abre sin internet, cae a Georgia / system-ui sin romperse. -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600&family=Nunito:wght@600;700;800&display=swap" rel="stylesheet">

<style>
:root{
  /* Paleta atardecer */
  --sky-1:#241a3a; --sky-2:#3a2350; --sky-3:#6e2f5e; --sky-4:#b8455f;
  --sky-5:#e86a4b; --sky-6:#f6a552; --sky-7:#ffd089;
  --floor-1:#4a2338; --floor-2:#341829;
  --curtain-a:#7d2536; --curtain-b:#5a1826; --curtain-c:#6b1f2e;
  --gold:#e9c46a;
  --text:#fff4e6; --text-soft:#ffd6a3;
  /* Personaje */
  --ink:#43302b; --skin:#ffe0c7; --coral:#e07a5f; --hair:#4a2f2a;
  --serif:'Fraunces','Georgia','Times New Roman',serif;
  --round:'Nunito',ui-rounded,'Segoe UI',system-ui,-apple-system,sans-serif;
}

*{ box-sizing:border-box; }
html,body{ height:100%; margin:0; overflow:hidden; }
body{
  background:var(--sky-1);
  font-family:var(--round);
  color:var(--text);
  -webkit-tap-highlight-color:transparent;
  -webkit-font-smoothing:antialiased;
  user-select:none;
}

/* ===================== ESCENARIO (raíz) ===================== */
.theater{
  position:fixed; inset:0; min-height:100dvh;
  overflow:hidden;
  background:var(--sky-1);
}

/* ------- Fondo: cielo, sol, montañas, foco, piso, luciérnagas ------- */
.backdrop{ position:absolute; inset:0; overflow:hidden; z-index:1; }

.sky{
  position:absolute; inset:0;
  background:linear-gradient(180deg,
    var(--sky-1) 0%, var(--sky-2) 17%, var(--sky-3) 36%,
    var(--sky-4) 56%, var(--sky-5) 73%, var(--sky-6) 88%, var(--sky-7) 100%);
}

.sun-glow{
  position:absolute; left:50%; bottom:4%;
  width:clamp(260px,62vmin,600px); aspect-ratio:1;
  transform:translateX(-50%); border-radius:50%;
  background:radial-gradient(circle,
    rgba(255,222,150,.55) 0%, rgba(255,190,120,.24) 36%, rgba(255,170,110,0) 68%);
  animation:sunPulse 8s ease-in-out infinite; will-change:opacity,transform;
}
.sun-core{
  position:absolute; left:50%; bottom:14%;
  width:clamp(120px,26vmin,240px); aspect-ratio:1;
  transform:translateX(-50%); border-radius:50%;
  background:radial-gradient(circle at 50% 44%, #fff6d8 0%, #ffdd92 55%, #ffca79 100%);
}
@keyframes sunPulse{
  0%,100%{ opacity:.78; transform:translateX(-50%) scale(1); }
  50%{ opacity:1; transform:translateX(-50%) scale(1.05); }
}

.hills{ position:absolute; left:0; bottom:clamp(70px,14vh,140px); width:100%; height:26%; }
.hills svg{ display:block; width:100%; height:100%; }

.floor{
  position:absolute; left:0; bottom:0; width:100%;
  height:clamp(70px,14vh,140px);
  background:linear-gradient(180deg,var(--floor-1) 0%, var(--floor-2) 100%);
  box-shadow:inset 0 2px 0 rgba(255,205,150,.22);
  z-index:2;
}

.spotlight{
  position:absolute; top:0; left:50%; transform:translateX(-50%);
  width:72%; height:100%;
  background:linear-gradient(180deg, rgba(255,244,214,.20), rgba(255,244,214,0) 70%);
  clip-path:polygon(42% 0, 58% 0, 82% 100%, 18% 100%);
  mix-blend-mode:screen; pointer-events:none;
}

.warmth{
  position:absolute; inset:0; pointer-events:none;
  background:radial-gradient(circle at 50% 60%, rgba(255,204,140,.38), rgba(255,150,90,0) 60%);
  opacity:0; transition:opacity 1.3s ease;
}
.scene.finale .warmth{ opacity:1; }

.mote{
  position:absolute; bottom:-6%; border-radius:50%;
  background:radial-gradient(circle,#ffe9b0,rgba(255,210,120,0) 70%);
  opacity:0; pointer-events:none;
  animation:moteRise linear infinite; will-change:transform,opacity;
}
@keyframes moteRise{
  0%{ transform:translate(0,0); opacity:0; }
  12%{ opacity:.8; }
  88%{ opacity:.55; }
  100%{ transform:translate(var(--dx,10px),-94vh); opacity:0; }
}

/* ===================== CONTENIDO (columna flex) ===================== */
.content{ position:absolute; inset:0; z-index:10; height:100%; display:flex; flex-direction:column; }

.scene{ flex:1 1 auto; position:relative; min-height:0; }

/* Lienzo de brasas del final */
.fx{ position:absolute; inset:0; z-index:15; pointer-events:none; }

/* ------- Actores: personaje + micrófono ------- */
.performers{
  position:absolute; left:50%; bottom:clamp(58px,12.5vh,128px);
  transform:translateX(-50%);
  display:flex; align-items:flex-end;
  z-index:14;
}

.mic{
  height:clamp(160px,42vmin,320px); width:auto;
  margin-left:clamp(-46px,-9vmin,-18px);
  transform:translateY(-3%);
}
.mic svg{ display:block; height:100%; width:auto; }

.stage-character{
  position:relative;
  width:clamp(150px,40vmin,300px);
  transform:translateX(-58vw);
  transition:transform 2.25s cubic-bezier(.33,0,.2,1);
  will-change:transform;
}
.stage-character.entered{ transform:translateX(0); }
.stage-character.no-anim{ transition:none !important; }

.shadow{
  position:absolute; left:50%; bottom:-2%;
  width:78%; height:8%;
  transform:translateX(-50%);
  background:radial-gradient(ellipse at center, rgba(30,10,20,.42), rgba(30,10,20,0) 70%);
  border-radius:50%;
}

.char-bob{ will-change:transform; }
.char-svg{ display:block; width:100%; height:auto; overflow:visible; }
@keyframes bob{ 0%,100%{ transform:translateY(0); } 50%{ transform:translateY(-5px); } }
.stage-character.walking .char-bob{ animation:bob .5s ease-in-out infinite; }

/* ---- Piezas del personaje (líneas y rellenos) ---- */
.head,.neck{ fill:var(--skin); stroke:var(--ink); stroke-width:6; }
.hair{ fill:var(--hair); }
.shirt{ fill:var(--coral); stroke:var(--ink); stroke-width:6; stroke-linejoin:round; }
.limb-ink{ stroke:var(--ink); stroke-width:17; stroke-linecap:round; fill:none; }
.limb-skin{ stroke:var(--skin); stroke-width:11; stroke-linecap:round; fill:none; }
.brows path{ stroke:var(--ink); stroke-width:4.5; stroke-linecap:round; fill:none; }
.eyes circle{ fill:var(--ink); }
.eyes path{ stroke:var(--ink); stroke-width:5; stroke-linecap:round; fill:none; }
.mouth{ stroke:var(--ink); stroke-width:6; stroke-linecap:round; fill:none; }
.m-grin{ fill:var(--ink); stroke:none; }
.sweat{ fill:#bfe6ff; stroke:#7fb8e0; stroke-width:1.5; }
.fly-heart{ fill:#ff5d73; }

/* Estados de cara: todo oculto y se revela por ánimo (crossfade) */
.eyes,.mouth,.sweat,.fly-heart{ opacity:0; transition:opacity .3s ease; }

/* Ejes de rotación (coordenadas del viewBox) */
.brows{ transform-box:view-box; transform-origin:100px 76px; transition:transform .3s ease; }
.arm{ transform-box:view-box; transition:transform .35s ease; }
.arm-left{ transform-origin:62px 150px; transform:rotate(6deg); }
.arm-right{ transform-origin:138px 150px; transform:rotate(-6deg); }
.leg{ transform-box:view-box; }
.leg-left{ transform-origin:86px 236px; }
.leg-right{ transform-origin:114px 236px; }
.fly-heart{ transform-box:view-box; transform-origin:100px 150px; }
.sweat{ transform-box:view-box; }

/* Caminar */
@keyframes stepA{ 0%,100%{ transform:rotate(20deg);} 50%{ transform:rotate(-20deg);} }
@keyframes stepB{ 0%,100%{ transform:rotate(-20deg);} 50%{ transform:rotate(20deg);} }
@keyframes armA{ 0%,100%{ transform:rotate(16deg);} 50%{ transform:rotate(-4deg);} }
@keyframes armB{ 0%,100%{ transform:rotate(-16deg);} 50%{ transform:rotate(4deg);} }
.stage-character.walking .leg-left{ animation:stepA .5s ease-in-out infinite; }
.stage-character.walking .leg-right{ animation:stepB .5s ease-in-out infinite; }
.stage-character.walking .arm-left{ animation:armA .5s ease-in-out infinite; }
.stage-character.walking .arm-right{ animation:armB .5s ease-in-out infinite; }

/* Ánimos por paso */
.character[data-mood="curious"] .eyes-open,
.character[data-mood="warm"] .eyes-open,
.character[data-mood="sincere"] .eyes-open,
.character[data-mood="sheepish"] .eyes-open{ opacity:1; }
.character[data-mood="celebrate"] .eyes-happy,
.character[data-mood="grateful"] .eyes-happy,
.character[data-mood="heart"] .eyes-happy,
.character[data-mood="joy"] .eyes-happy{ opacity:1; }
.character[data-mood="playful"] .eyes-wink{ opacity:1; }

.character[data-mood="curious"] .m-smile,
.character[data-mood="warm"] .m-smile,
.character[data-mood="sincere"] .m-smile{ opacity:1; }
.character[data-mood="playful"] .m-smirk{ opacity:1; }
.character[data-mood="celebrate"] .m-grin,
.character[data-mood="joy"] .m-grin{ opacity:1; }
.character[data-mood="grateful"] .m-soft,
.character[data-mood="heart"] .m-soft{ opacity:1; }
.character[data-mood="sheepish"] .m-squig{ opacity:1; }

.character[data-mood="curious"] .brows{ transform:translateY(-3px); }

/* Gotita de pena (paso 4) */
@keyframes drip{ 0%,100%{ transform:translateY(0);} 50%{ transform:translateY(3px);} }
.character[data-mood="sheepish"] .sweat{ opacity:1; animation:drip 1.3s ease-in-out infinite; }

/* Mano al corazón (paso 8) + corazón que sube */
.character[data-mood="heart"] .arm-left{ transform:rotate(52deg); }
@keyframes heartFloat{
  0%{ opacity:0; transform:translate(0,0) scale(.6);}
  22%{ opacity:1;}
  100%{ opacity:0; transform:translate(4px,-70px) scale(1.12);}
}
.character[data-mood="heart"] .fly-heart{ animation:heartFloat 1.8s ease-out forwards; }

/* Saludo final (paso 9) */
@keyframes wave{ 0%,100%{ transform:rotate(-150deg);} 50%{ transform:rotate(-172deg);} }
.character[data-mood="joy"] .arm-right{ animation:wave .7s ease-in-out infinite; }

/* ===================== TEXTO ===================== */
.caption-band{
  flex:0 0 auto; min-height:22vh;
  display:flex; flex-direction:column; align-items:center; justify-content:center;
  gap:1vh; padding:1.4vh 6vw; position:relative; z-index:20; text-align:center;
}
.caption{
  max-width:22ch;
  font-family:var(--serif);
  font-weight:500; line-height:1.34;
  font-size:clamp(1.2rem,4.7vw,2.1rem);
  color:var(--text);
  text-shadow:0 2px 14px rgba(40,10,20,.55);
  text-wrap:balance;
  opacity:0; transform:translateY(15px);
  transition:opacity .34s ease, transform .34s ease;
  will-change:opacity,transform;
}
.caption.show{ opacity:1; transform:none; }
.caption.final{
  font-size:clamp(1.55rem,6.4vw,3rem);
  font-weight:600;
  background:linear-gradient(92deg,#ffe0a8,#ffb37a 55%,#ff9d8a);
  -webkit-background-clip:text; background-clip:text;
  -webkit-text-fill-color:transparent; color:transparent;
  text-shadow:none;
}
.signature{
  font-family:var(--serif); font-style:italic;
  font-size:clamp(1rem,3.6vw,1.4rem);
  color:var(--text-soft); letter-spacing:.04em;
  opacity:0; transform:translateY(8px);
  transition:opacity .55s ease .25s, transform .55s ease .25s;
}
.signature.show{ opacity:1; transform:none; }

/* ===================== CONTROLES ===================== */
.controls{
  flex:0 0 auto;
  display:flex; flex-direction:column; align-items:center; gap:1.5vh;
  padding:0 6vw calc(2.4vh + env(safe-area-inset-bottom,0px));
  z-index:20;
  opacity:0; transition:opacity .5s ease;
}
.controls.ready{ opacity:1; }

.dots{ display:flex; gap:8px; }
.dot{
  width:9px; height:9px; border-radius:50%;
  background:rgba(255,220,180,.28);
  transition:background .3s ease, transform .3s ease;
}
.dot.on{ background:#ffcf8a; transform:scale(1.18); }

.next{
  appearance:none; border:none; cursor:pointer; font-family:var(--round);
  font-weight:800; letter-spacing:.02em; color:#4a261b;
  padding:.85em 1.9em; border-radius:999px;
  background:linear-gradient(180deg,#ffd89b,#f6a552);
  box-shadow:0 6px 18px rgba(180,90,40,.35), inset 0 1px 0 rgba(255,255,255,.55);
  transition:transform .12s ease, filter .2s ease;
  touch-action:manipulation;
}
.next:hover{ filter:brightness(1.05); }
.next:active{ transform:scale(.95); }
.next:focus-visible{ outline:3px solid #fff2d6; outline-offset:3px; }

/* ===================== CORTINAS + CENEFA ===================== */
.curtain{
  position:absolute; top:0; bottom:0; width:56%; z-index:40;
  transition:transform 1.15s cubic-bezier(.7,0,.15,1);
  will-change:transform;
}
.curtain-l{
  left:0;
  background:repeating-linear-gradient(90deg,#6b1f2e 0px,#7d2536 22px,#5a1826 44px);
  box-shadow:inset -22px 0 44px rgba(0,0,0,.42);
}
.curtain-r{
  right:0;
  background:repeating-linear-gradient(90deg,#5a1826 0px,#7d2536 22px,#6b1f2e 44px);
  box-shadow:inset 22px 0 44px rgba(0,0,0,.42);
}
.theater.open .curtain-l{ transform:translateX(-84%); }
.theater.open .curtain-r{ transform:translateX(84%); }

.valance{ position:absolute; top:0; left:0; width:100%; height:clamp(46px,9vh,92px); z-index:41; }
.valance svg{ display:block; width:100%; height:100%; }

/* ===================== PANTALLA DE INICIO ===================== */
.start{
  position:absolute; inset:0; z-index:50;
  display:flex; flex-direction:column; align-items:center; justify-content:center;
  gap:2.2vh; text-align:center; padding:8vw; cursor:pointer;
  background:radial-gradient(circle at 50% 38%, rgba(40,20,50,.15), rgba(18,9,24,.78));
  -webkit-backdrop-filter:blur(2px); backdrop-filter:blur(2px);
  transition:opacity .6s ease;
}
.start.hidden{ opacity:0; pointer-events:none; }
.start .eyebrow{
  font-family:var(--round); font-weight:700; letter-spacing:.34em;
  text-transform:uppercase; font-size:clamp(.7rem,2.8vw,.9rem);
  color:var(--text-soft); opacity:.9;
}
.start h1{
  margin:0; font-family:var(--serif); font-weight:500;
  font-size:clamp(1.9rem,7.5vw,3.6rem); line-height:1.05; color:var(--text);
  text-shadow:0 2px 20px rgba(40,10,20,.5);
}
.start .who{
  font-family:var(--serif); font-style:italic; font-weight:600;
  font-size:clamp(2.2rem,10vw,4.4rem); line-height:1;
  background:linear-gradient(92deg,#ffe0a8,#ffb37a 55%,#ff9d8a);
  -webkit-background-clip:text; background-clip:text; -webkit-text-fill-color:transparent; color:transparent;
}
.start .hint{
  margin-top:1.5vh; font-family:var(--round); font-weight:700;
  font-size:clamp(.95rem,3.6vw,1.2rem); color:var(--text);
  animation:pulse 2.2s ease-in-out infinite;
}
@keyframes pulse{ 0%,100%{ opacity:.5; } 50%{ opacity:1; } }
.begin-btn{
  margin-top:.5vh;
  appearance:none; border:none; cursor:pointer; font-family:var(--round);
  font-weight:800; color:#4a261b; letter-spacing:.02em;
  padding:.9em 2.2em; border-radius:999px;
  background:linear-gradient(180deg,#ffe1ad,#f4a259);
  box-shadow:0 8px 22px rgba(180,90,40,.4), inset 0 1px 0 rgba(255,255,255,.6);
  transition:transform .12s ease, filter .2s ease;
  touch-action:manipulation;
}
.begin-btn:active{ transform:scale(.95); }
.begin-btn:focus-visible{ outline:3px solid #fff2d6; outline-offset:3px; }

.spark{ position:absolute; color:#ffe0a0; font-size:clamp(10px,2vw,16px); opacity:.1; animation:tw 3.4s ease-in-out infinite; }
@keyframes tw{ 0%,100%{ opacity:.08; transform:scale(.8);} 50%{ opacity:.85; transform:scale(1.12);} }

/* ===================== ACCESIBILIDAD: menos movimiento ===================== */
@media (prefers-reduced-motion: reduce){
  .theater *{ animation-duration:.001s !important; animation-iteration-count:1 !important; }
  .stage-character{ transition-duration:.001s !important; }
  .caption{ transition-duration:.18s !important; }
  .curtain{ transition-duration:.4s !important; }
}
</style>
</head>

<body>
<div class="theater" id="theater">

  <!-- Fondo -->
  <div class="backdrop" id="backdrop">
    <div class="sky"></div>
    <div class="sun-glow"></div>
    <div class="sun-core"></div>
    <div class="hills">
      <svg viewBox="0 0 100 30" preserveAspectRatio="none">
        <path d="M0,30 L0,14 Q18,4 34,12 Q52,20 68,9 Q84,1 100,11 L100,30 Z" fill="#5e2b46"/>
        <path d="M0,30 L0,20 Q22,12 40,18 Q60,25 78,16 Q90,11 100,18 L100,30 Z" fill="#3d1d33"/>
      </svg>
    </div>
    <div class="floor"></div>
    <!-- Las luciérnagas (.mote) se inyectan por JS aquí -->
  </div>

  <!-- Contenido en columna -->
  <div class="content">
    <div class="scene" id="scene">
      <div class="spotlight"></div>
      <div class="warmth"></div>
      <canvas class="fx" id="fx"></canvas>

      <div class="performers">
        <!-- Personaje -->
        <div class="stage-character character" id="character">
          <div class="shadow"></div>
          <div class="char-bob">
            <svg class="char-svg" viewBox="0 0 200 314" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
              <!-- Piernas -->
              <g class="leg leg-left">
                <line class="limb-ink"  x1="86" y1="238" x2="84" y2="302"/>
                <line class="limb-skin" x1="86" y1="238" x2="84" y2="302"/>
              </g>
              <g class="leg leg-right">
                <line class="limb-ink"  x1="114" y1="238" x2="116" y2="302"/>
                <line class="limb-skin" x1="114" y1="238" x2="116" y2="302"/>
              </g>
              <!-- Cuello -->
              <path class="neck" d="M89,126 h22 v20 h-22 Z"/>
              <!-- Torso -->
              <path class="shirt" d="M62,150 Q100,140 138,150 L130,236 Q100,246 70,236 Z"/>
              <!-- Cabeza -->
              <circle class="head" cx="100" cy="88" r="46"/>
              <!-- Pelo con flequillo -->
              <path class="hair" d="M54,88 A46,46 0 0 1 146,88
                C146,80 138,76 128,80 C124,72 112,74 108,80
                C104,72 96,72 92,80 C88,74 76,72 72,80
                C62,76 54,80 54,88 Z"/>
              <!-- Brazos (encima del torso para que la mano se vea) -->
              <g class="arm arm-left">
                <line class="limb-ink"  x1="62" y1="152" x2="54" y2="208"/>
                <line class="limb-skin" x1="62" y1="152" x2="54" y2="208"/>
              </g>
              <g class="arm arm-right">
                <line class="limb-ink"  x1="138" y1="152" x2="146" y2="208"/>
                <line class="limb-skin" x1="138" y1="152" x2="146" y2="208"/>
              </g>
              <!-- Cejas -->
              <g class="brows">
                <path d="M76,76 Q84,72 92,76"/>
                <path d="M108,76 Q116,72 124,76"/>
              </g>
              <!-- Ojos: 3 variantes -->
              <g class="eyes eyes-open">
                <circle cx="84" cy="90" r="6"/>
                <circle cx="116" cy="90" r="6"/>
              </g>
              <g class="eyes eyes-happy">
                <path d="M78,91 Q84,84 90,91"/>
                <path d="M110,91 Q116,84 122,91"/>
              </g>
              <g class="eyes eyes-wink">
                <circle cx="84" cy="90" r="6"/>
                <path d="M110,91 Q116,84 122,91"/>
              </g>
              <!-- Bocas: 5 variantes -->
              <path class="mouth m-smile"  d="M84,110 Q100,124 116,110"/>
              <path class="mouth m-grin"   d="M82,108 Q100,116 118,108 Q100,136 82,108 Z"/>
              <path class="mouth m-soft"   d="M90,113 Q100,120 110,113"/>
              <path class="mouth m-squig"  d="M86,114 Q92,108 98,114 Q104,120 110,114"/>
              <path class="mouth m-smirk"  d="M86,113 Q100,121 116,106"/>
              <!-- Gotita de pena -->
              <path class="sweat" d="M148,64 c5,8 5,12 0,16 c-5,-4 -5,-8 0,-16 Z"/>
              <!-- Corazón que flota -->
              <path class="fly-heart" d="M100,150 c-6,-8 -18,-2 -14,7 c3,7 14,13 14,13 c0,0 11,-6 14,-13 c4,-9 -8,-15 -14,-7 Z"/>
            </svg>
          </div>
        </div>

        <!-- Micrófono -->
        <div class="mic">
          <svg viewBox="0 0 80 240" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
            <ellipse cx="40" cy="236" rx="26" ry="7" fill="#2a2a30"/>
            <line x1="40" y1="235" x2="40" y2="82" stroke="#2a2a30" stroke-width="7" stroke-linecap="round"/>
            <rect x="36" y="70" width="8" height="14" rx="3" fill="#2a2a30"/>
            <rect x="24" y="16" width="32" height="58" rx="16" fill="#3a3a42" stroke="#1f1f24" stroke-width="3"/>
            <g stroke="#1f1f24" stroke-width="2">
              <line x1="30" y1="30" x2="50" y2="30"/>
              <line x1="30" y1="38" x2="50" y2="38"/>
              <line x1="30" y1="46" x2="50" y2="46"/>
              <line x1="30" y1="54" x2="50" y2="54"/>
              <line x1="30" y1="62" x2="50" y2="62"/>
            </g>
            <rect x="30" y="22" width="7" height="30" rx="3.5" fill="rgba(255,255,255,.16)"/>
          </svg>
        </div>
      </div>
    </div>

    <!-- Texto de la historia -->
    <div class="caption-band">
      <p class="caption" id="caption"></p>
      <p class="signature" id="signature">— Para ti, Adriana ✦</p>
    </div>

    <!-- Controles -->
    <div class="controls">
      <div class="dots" id="dots"></div>
      <button class="next" id="nextBtn" type="button">Siguiente ›</button>
    </div>
  </div>

  <!-- Cortinas y cenefa (encima de todo, se abren al empezar) -->
  <div class="curtain curtain-l"></div>
  <div class="curtain curtain-r"></div>
  <div class="valance">
    <svg viewBox="0 0 120 20" preserveAspectRatio="none" xmlns="http://www.w3.org/2000/svg">
      <defs>
        <linearGradient id="vg" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0" stop-color="#7d2536"/>
          <stop offset="1" stop-color="#591826"/>
        </linearGradient>
      </defs>
      <path fill="url(#vg)" d="M0,0 H120 V11
        q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0
        q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 V0 Z"/>
      <path fill="none" stroke="#e9c46a" stroke-width="1.1" d="M120,11
        q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0
        q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0 q-6,7 -12,0"/>
    </svg>
  </div>

  <!-- Telón de inicio -->
  <div class="start" id="startScreen" role="button" tabindex="0" aria-label="Comenzar la función">
    <span class="spark" style="top:18%; left:16%; animation-delay:0s">✦</span>
    <span class="spark" style="top:26%; left:80%; animation-delay:.9s">✧</span>
    <span class="spark" style="top:70%; left:22%; animation-delay:1.6s">✧</span>
    <span class="spark" style="top:66%; left:78%; animation-delay:2.3s">✦</span>
    <div class="eyebrow">Una función especial</div>
    <h1>Un pequeño teatro<br>para</h1>
    <div class="who">Adriana</div>
    <p class="hint">✦ toca para abrir el telón ✦</p>
    <button class="begin-btn" id="beginBtn" type="button">Comenzar</button>
  </div>

  <!-- ============================================================= -->
  <!--  🎙️  TU VOZ AQUÍ (opcional)                                   -->
  <!--                                                               -->
  <!--  1. Guarda tu audio (ej: "voz.mp3") junto a este archivo.     -->
  <!--  2. Descomenta la línea de abajo y pon el nombre de tu archivo.-->
  <!--  3. Se reproducirá SOLO cuando la persona toque para          -->
  <!--     comenzar (así lo permiten los navegadores), y se          -->
  <!--     reinicia si vuelve a ver la función.                      -->
  <!--                                                               -->
  <!--  <audio id="voz" src="voz.mp3" preload="auto"></audio>        -->
  <!-- ============================================================= -->

</div>

<script>
(function(){
  "use strict";
  var $ = function(s){ return document.querySelector(s); };

  var theater   = $('#theater');
  var scene     = $('#scene');
  var backdrop  = $('#backdrop');
  var character = $('#character');
  var caption   = $('#caption');
  var signature = $('#signature');
  var dotsWrap  = $('#dots');
  var nextBtn   = $('#nextBtn');
  var startScr  = $('#startScreen');
  var beginBtn  = $('#beginBtn');
  var controls  = document.querySelector('.controls');
  var canvas    = $('#fx');
  var ctx       = canvas.getContext('2d');
  var reduce    = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  // ---- Historia ----
  var steps = [
    { text:"Te conocí de la nada, llegando al trabajo nuevo…",                 mood:"curious"  },
    { text:"…y nuestra primera charla fue que querías un aumento de sueldo.",  mood:"playful"  },
    { text:"De ahí, verte cumplir tus 30 años…",                              mood:"celebrate"},
    { text:"…y te pido perdón por darte un pastel de tres leches.",           mood:"sheepish" },
    { text:"Luego darnos esos regalos, empezar a conocerte…",                 mood:"warm"     },
    { text:"…y ya para este año te volviste mi apoyo incondicional.",         mood:"grateful" },
    { text:"Hoy quiero que sepas que esto no es un mensaje de «vuelve»…",     mood:"sincere"  },
    { text:"…es un regalo, de corazón, para ti.",                            mood:"heart"    },
    { text:"Ve, sé feliz y cómete el mundo.",                                mood:"joy", final:true }
  ];

  // ---- Puntitos de progreso ----
  var dots = [];
  steps.forEach(function(){
    var d = document.createElement('span');
    d.className = 'dot';
    dotsWrap.appendChild(d);
    dots.push(d);
  });

  // ---- Luciérnagas ambientales (CSS, baratas) ----
  var moteCount = reduce ? 0 : 12;
  for(var i=0;i<moteCount;i++){
    var m = document.createElement('span');
    m.className = 'mote';
    var size = 4 + Math.random()*6;
    m.style.left = (Math.random()*100) + '%';
    m.style.width = size + 'px';
    m.style.height = size + 'px';
    m.style.setProperty('--dx', (Math.random()*80 - 40) + 'px');
    var dur = 9 + Math.random()*8;
    m.style.animationDuration = dur + 's';
    m.style.animationDelay = (-Math.random()*dur) + 's';
    backdrop.appendChild(m);
  }

  var idx = -1, busy = false, started = false;
  var wait = function(ms){ return new Promise(function(r){ setTimeout(r, ms); }); };

  function voice(){ return document.getElementById('voz'); }
  function playVoice(){ var v=voice(); if(v){ try{ v.currentTime=0; if(v.play) v.play().catch(function(){}); }catch(e){} } }
  function stopVoice(){ var v=voice(); if(v){ try{ v.pause(); }catch(e){} } }

  function updateDots(){ for(var i=0;i<dots.length;i++){ dots[i].classList.toggle('on', i<=idx); } }

  function applyStep(i){
    idx = i;
    var s = steps[i];
    caption.textContent = s.text;
    caption.classList.toggle('final', !!s.final);
    character.dataset.mood = s.mood;
    updateDots();
    if(s.final){
      nextBtn.textContent = 'Repetir función ↺';
      signature.classList.add('show');
      scene.classList.add('finale');
      startFx();
    }
  }

  async function goTo(i){
    busy = true;
    caption.classList.remove('show');
    signature.classList.remove('show');
    await wait(reduce ? 40 : 330);
    applyStep(i);
    void caption.offsetWidth;              // reinicia la transición
    caption.classList.add('show');
    await wait(reduce ? 40 : 360);
    busy = false;
  }

  async function begin(){
    if(started || busy) return;
    started = true; busy = true;
    playVoice();
    startScr.classList.add('hidden');
    await wait(reduce ? 20 : 220);
    theater.classList.add('open');                 // se abren las cortinas
    await wait(reduce ? 20 : 850);
    character.dataset.mood = 'curious';            // cara amable al entrar
    if(!reduce) character.classList.add('walking');
    requestAnimationFrame(function(){ character.classList.add('entered'); }); // camina al micro
    await wait(reduce ? 60 : 2250);
    character.classList.remove('walking');
    await wait(reduce ? 20 : 150);
    controls.classList.add('ready');
    busy = false;
    goTo(0);
  }

  async function replay(){
    busy = true;
    stopFx();
    scene.classList.remove('finale');
    caption.classList.remove('show');
    signature.classList.remove('show');
    await wait(300);
    theater.classList.remove('open');             // se cierran las cortinas
    await wait(reduce ? 60 : 950);
    // Regresa al personaje fuera de escena sin animación (oculto tras las cortinas)
    character.classList.add('no-anim');
    character.classList.remove('entered');
    character.removeAttribute('data-mood');
    void character.offsetWidth;
    character.classList.remove('no-anim');
    idx = -1; updateDots();
    nextBtn.textContent = 'Siguiente ›';
    controls.classList.remove('ready');
    stopVoice();
    started = false;
    startScr.classList.remove('hidden');
    busy = false;
  }

  function advance(){
    if(busy) return;
    if(idx >= steps.length - 1) replay();
    else goTo(idx + 1);
  }

  nextBtn.addEventListener('click', advance);
  beginBtn.addEventListener('click', function(e){ e.stopPropagation(); begin(); });
  startScr.addEventListener('click', begin);
  startScr.addEventListener('keydown', function(e){
    if(e.key === 'Enter' || e.key === ' '){ e.preventDefault(); begin(); }
  });

  /* ---------------- Brasas del gran final (canvas) ---------------- */
  var parts = [], raf = null, emitting = false, emitUntil = 0, dpr = 1;

  function sizeCanvas(){
    var r = scene.getBoundingClientRect();
    dpr = Math.min(window.devicePixelRatio || 1, 2);
    canvas.width  = Math.max(1, Math.floor(r.width  * dpr));
    canvas.height = Math.max(1, Math.floor(r.height * dpr));
    canvas.style.width  = r.width  + 'px';
    canvas.style.height = r.height + 'px';
  }

  function spawn(){
    var colors = ['#ffd98a','#ffb877','#ff9d7a','#ffe6b0','#ffcf6a'];
    parts.push({
      x: canvas.width * (0.2 + Math.random()*0.6),
      y: canvas.height * (0.9 + Math.random()*0.12),
      vx: (Math.random()-0.5) * 0.5 * dpr,
      vy: -(0.6 + Math.random()*1.1) * dpr,
      r: (2 + Math.random()*3.5) * dpr,
      life: 0, ttl: 120 + Math.random()*80,
      c: colors[(Math.random()*colors.length)|0],
      sway: Math.random()*Math.PI*2
    });
  }

  function tick(t){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    if(emitting){
      if(t < emitUntil){ for(var s=0;s<3;s++) spawn(); }
      else emitting = false;
    }
    for(var i=parts.length-1;i>=0;i--){
      var p = parts[i];
      p.life++; p.sway += 0.05;
      p.x += p.vx + Math.sin(p.sway)*0.3*dpr;
      p.y += p.vy; p.vy += 0.004*dpr;
      var k = 1 - p.life/p.ttl;
      if(k <= 0 || p.y < -20){ parts.splice(i,1); continue; }
      ctx.globalAlpha = Math.max(0, Math.min(1, k)) * 0.9;
      ctx.fillStyle = p.c;
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, 6.2832);
      ctx.fill();
    }
    ctx.globalAlpha = 1;
    if(parts.length > 0 || emitting){ raf = requestAnimationFrame(tick); }
    else { raf = null; }
  }

  function startFx(){
    if(reduce) return;
    sizeCanvas();
    emitting = true;
    emitUntil = performance.now() + 1600;
    if(!raf) raf = requestAnimationFrame(tick);
  }
  function stopFx(){
    emitting = false; parts = [];
    if(raf){ cancelAnimationFrame(raf); raf = null; }
    if(ctx) ctx.clearRect(0,0,canvas.width,canvas.height);
  }

  window.addEventListener('resize', function(){
    if(scene.classList.contains('finale')) sizeCanvas();
  });
})();
</script>
</body>
</html>
