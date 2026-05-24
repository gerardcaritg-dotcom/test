<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Test Organización Profesional</title>
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0e0e14;
    --card: #18181f;
    --card2: #1f1f28;
    --border: #2c2c3a;
    --gold: #d4a843;
    --purple: #7b6fd4;
    --text: #eeedf5;
    --muted: #7a7a90;
    --green-bg: #162b20;
    --green-border: #4caf82;
    --green-text: #90dfb8;
    --red-bg: #2b1616;
    --red-border: #d46060;
    --red-text: #f0a0a0;
    --r: 16px;
    --safe-bottom: env(safe-area-inset-bottom, 16px);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }

  html, body {
    height: 100%;
    overflow: hidden;
    background: var(--bg);
    color: var(--text);
    font-family: 'Sora', sans-serif;
    font-size: 15px;
  }

  /* ── LAYOUT ── */
  #app {
    display: flex;
    flex-direction: column;
    height: 100%;
    max-width: 480px;
    margin: 0 auto;
  }

  /* ── HEADER ── */
  #topbar {
    flex-shrink: 0;
    padding: 14px 18px 10px;
    background: var(--bg);
    border-bottom: 1px solid var(--border);
  }

  .top-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;
  }

  .app-title {
    font-size: 0.7rem;
    font-weight: 700;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: var(--gold);
  }

  .score-chips {
    display: flex;
    gap: 8px;
  }

  .chip {
    font-size: 0.75rem;
    font-weight: 700;
    padding: 3px 10px;
    border-radius: 999px;
    border: 1px solid;
  }
  .chip-ok  { color: var(--green-text); border-color: var(--green-border); background: var(--green-bg); }
  .chip-bad { color: var(--red-text);   border-color: var(--red-border);   background: var(--red-bg); }

  /* progress row */
  .prog-row {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .prog-track {
    flex: 1;
    height: 5px;
    background: var(--border);
    border-radius: 999px;
    overflow: hidden;
  }
  .prog-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--purple), var(--gold));
    border-radius: 999px;
    transition: width 0.4s ease;
  }
  .prog-label {
    font-size: 0.72rem;
    font-weight: 600;
    color: var(--muted);
    white-space: nowrap;
  }

  /* section badge */
  #section-badge {
    display: inline-block;
    margin-top: 8px;
    font-size: 0.66rem;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--purple);
    background: rgba(123,111,212,0.12);
    border: 1px solid rgba(123,111,212,0.3);
    padding: 3px 10px;
    border-radius: 999px;
  }

  /* ── CARD VIEWPORT ── */
  #card-viewport {
    flex: 1;
    overflow: hidden;
    position: relative;
  }

  /* ── QUESTION CARD ── */
  .q-slide {
    position: absolute;
    inset: 0;
    padding: 20px 18px 16px;
    display: flex;
    flex-direction: column;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    transition: transform 0.35s cubic-bezier(0.4,0,0.2,1), opacity 0.35s ease;
  }
  .q-slide.enter-right  { transform: translateX(100%); opacity: 0; }
  .q-slide.enter-left   { transform: translateX(-100%); opacity: 0; }
  .q-slide.active       { transform: translateX(0); opacity: 1; }
  .q-slide.exit-left    { transform: translateX(-100%); opacity: 0; }
  .q-slide.exit-right   { transform: translateX(100%); opacity: 0; }

  .q-num {
    font-size: 0.65rem;
    font-weight: 700;
    letter-spacing: 0.14em;
    text-transform: uppercase;
    color: var(--gold);
    margin-bottom: 10px;
  }

  .q-text {
    font-size: 1rem;
    font-weight: 600;
    line-height: 1.55;
    color: var(--text);
    margin-bottom: 20px;
  }

  .options {
    display: flex;
    flex-direction: column;
    gap: 10px;
    flex: 1;
  }

  .opt-btn {
    background: var(--card);
    border: 1.5px solid var(--border);
    border-radius: 14px;
    padding: 14px 16px;
    display: flex;
    align-items: flex-start;
    gap: 13px;
    text-align: left;
    cursor: pointer;
    color: var(--text);
    font-family: 'Sora', sans-serif;
    font-size: 0.88rem;
    line-height: 1.5;
    transition: background 0.18s, border-color 0.18s, transform 0.1s;
    width: 100%;
    -webkit-appearance: none;
    appearance: none;
    touch-action: manipulation;
  }
  .opt-btn:active:not(:disabled) {
    transform: scale(0.97);
    background: var(--card2);
  }
  .opt-btn:disabled { cursor: default; }

  .opt-circle {
    flex-shrink: 0;
    width: 28px;
    height: 28px;
    border-radius: 50%;
    background: var(--border);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.72rem;
    font-weight: 800;
    color: var(--muted);
    transition: background 0.2s, color 0.2s;
    margin-top: 1px;
  }

  .opt-btn.correct {
    background: var(--green-bg);
    border-color: var(--green-border);
    color: var(--green-text);
  }
  .opt-btn.correct .opt-circle {
    background: var(--green-border);
    color: #fff;
  }

  .opt-btn.incorrect {
    background: var(--red-bg);
    border-color: var(--red-border);
    color: var(--red-text);
  }
  .opt-btn.incorrect .opt-circle {
    background: var(--red-border);
    color: #fff;
  }

  /* feedback banner */
  .feedback-banner {
    margin-top: 14px;
    padding: 11px 14px;
    border-radius: 12px;
    font-size: 0.82rem;
    font-weight: 600;
    display: none;
    align-items: center;
    gap: 8px;
  }
  .feedback-banner.show { display: flex; }
  .feedback-banner.ok   { background: rgba(76,175,130,0.14); color: var(--green-text); border: 1px solid rgba(76,175,130,0.3); }
  .feedback-banner.fail { background: rgba(212,96,96,0.14);  color: var(--red-text);   border: 1px solid rgba(212,96,96,0.3); }
  .fb-icon { font-size: 1.1rem; flex-shrink: 0; }

  /* ── BOTTOM NAV ── */
  #bottombar {
    flex-shrink: 0;
    padding: 12px 18px calc(12px + var(--safe-bottom));
    background: var(--bg);
    border-top: 1px solid var(--border);
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .nav-btn {
    flex: 1;
    padding: 14px 10px;
    border-radius: 14px;
    border: 1.5px solid var(--border);
    background: var(--card);
    color: var(--muted);
    font-family: 'Sora', sans-serif;
    font-size: 0.85rem;
    font-weight: 700;
    cursor: pointer;
    transition: background 0.18s, border-color 0.18s, color 0.18s;
    touch-action: manipulation;
  }
  .nav-btn:disabled { opacity: 0.3; cursor: default; }
  .nav-btn:not(:disabled):active { transform: scale(0.96); }

  #btn-next {
    flex: 2;
    background: linear-gradient(135deg, var(--purple), var(--gold));
    border-color: transparent;
    color: #fff;
  }
  #btn-next:disabled {
    background: var(--card);
    color: var(--muted);
    border-color: var(--border);
    opacity: 0.5;
  }

  /* ── RESULTS SCREEN ── */
  #results-screen {
    display: none;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 32px 24px;
    text-align: center;
    height: 100%;
  }
  #results-screen.show { display: flex; }

  .result-ring {
    width: 140px;
    height: 140px;
    border-radius: 50%;
    background: conic-gradient(var(--gold) 0%, var(--purple) 50%, var(--border) 50%);
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 24px;
    position: relative;
  }
  .result-ring::before {
    content: '';
    position: absolute;
    inset: 10px;
    background: var(--bg);
    border-radius: 50%;
  }
  .result-pct {
    position: relative;
    z-index: 1;
    font-size: 1.8rem;
    font-weight: 800;
    background: linear-gradient(135deg, var(--gold), var(--purple));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .result-title {
    font-size: 1.3rem;
    font-weight: 800;
    color: var(--text);
    margin-bottom: 8px;
  }
  .result-detail {
    font-size: 0.88rem;
    color: var(--muted);
    line-height: 1.6;
    margin-bottom: 32px;
  }
  .stat-row {
    display: flex;
    gap: 14px;
    margin-bottom: 32px;
  }
  .stat-box {
    flex: 1;
    padding: 16px;
    border-radius: 14px;
    border: 1.5px solid;
    text-align: center;
  }
  .stat-box.ok  { background: var(--green-bg); border-color: var(--green-border); }
  .stat-box.bad { background: var(--red-bg);   border-color: var(--red-border); }
  .stat-num {
    font-size: 1.8rem;
    font-weight: 800;
    line-height: 1;
    margin-bottom: 4px;
  }
  .stat-box.ok  .stat-num { color: var(--green-text); }
  .stat-box.bad .stat-num { color: var(--red-text); }
  .stat-lbl {
    font-size: 0.72rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--muted);
  }

  .restart-btn {
    width: 100%;
    padding: 16px;
    border-radius: 14px;
    border: none;
    background: linear-gradient(135deg, var(--purple), var(--gold));
    color: #fff;
    font-family: 'Sora', sans-serif;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
    touch-action: manipulation;
  }
  .restart-btn:active { opacity: 0.88; transform: scale(0.97); }

  /* Splash screen */
  #splash {
    position: fixed;
    inset: 0;
    background: var(--bg);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 32px 28px calc(32px + var(--safe-bottom));
    text-align: center;
    z-index: 999;
  }
  .splash-icon { font-size: 3.5rem; margin-bottom: 20px; }
  .splash-title {
    font-size: 1.6rem;
    font-weight: 800;
    color: var(--text);
    margin-bottom: 8px;
  }
  .splash-sub {
    font-size: 0.9rem;
    color: var(--muted);
    line-height: 1.6;
    margin-bottom: 36px;
  }
  .splash-stats {
    display: flex;
    gap: 16px;
    margin-bottom: 36px;
    width: 100%;
  }
  .splash-stat {
    flex: 1;
    background: var(--card);
    border: 1.5px solid var(--border);
    border-radius: 14px;
    padding: 14px 10px;
  }
  .splash-stat-n {
    font-size: 1.5rem;
    font-weight: 800;
    color: var(--gold);
    line-height: 1;
    margin-bottom: 4px;
  }
  .splash-stat-l {
    font-size: 0.68rem;
    font-weight: 700;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--muted);
  }
  .start-btn {
    width: 100%;
    padding: 17px;
    border-radius: 14px;
    border: none;
    background: linear-gradient(135deg, var(--purple), var(--gold));
    color: #fff;
    font-family: 'Sora', sans-serif;
    font-size: 1.05rem;
    font-weight: 800;
    cursor: pointer;
    letter-spacing: 0.03em;
  }
</style>
</head>
<body>

<!-- Splash -->
<div id="splash">
  <div class="splash-icon">⚖️</div>
  <div class="splash-title">Test Oficial</div>
  <div class="splash-sub">Organización Profesional, Deontología<br>y Blanqueo de Capitales</div>
  <div class="splash-stats">
    <div class="splash-stat">
      <div class="splash-stat-n">84</div>
      <div class="splash-stat-l">Preguntas</div>
    </div>
    <div class="splash-stat">
      <div class="splash-stat-n">5</div>
      <div class="splash-stat-l">Bloques</div>
    </div>
    <div class="splash-stat">
      <div class="splash-stat-n">2023</div>
      <div class="splash-stat-l">Último exam</div>
    </div>
  </div>
  <button class="start-btn" onclick="startQuiz()">Empezar →</button>
</div>

<!-- Main app -->
<div id="app" style="display:none">
  <!-- Topbar -->
  <div id="topbar">
    <div class="top-row">
      <div class="app-title">⚖️ Test Oficial</div>
      <div class="score-chips">
        <div class="chip chip-ok">✓ <span id="sc-ok">0</span></div>
        <div class="chip chip-bad">✗ <span id="sc-bad">0</span></div>
      </div>
    </div>
    <div class="prog-row">
      <div class="prog-track"><div class="prog-fill" id="prog-fill" style="width:0%"></div></div>
      <div class="prog-label" id="prog-label">0 / 84</div>
    </div>
    <div id="section-badge">—</div>
  </div>

  <!-- Scrollable question area -->
  <div id="card-viewport">
    <div class="q-slide active" id="current-slide"></div>
    <div class="q-slide" id="next-slide" style="display:none"></div>
  </div>

  <!-- Bottom nav -->
  <div id="bottombar">
    <button class="nav-btn" id="btn-prev" onclick="goTo(currentIdx - 1)" disabled>← Ant.</button>
    <button class="nav-btn" id="btn-next" onclick="handleNext()" disabled>Siguiente →</button>
  </div>
</div>

<!-- Results overlay (inside app) -->
<div id="results-screen"></div>

<script>
const QUESTIONS = [
  // ───── EXAMEN FINAL 2023 ─────
  {
    section: "Examen Final 2023",
    text: "El ejercicio autónomo de la profesión de abogado:",
    options: [
      "No implica el ejercicio de la profesión con carácter habitual, directo y que genere al abogado su fuente principal de ingresos.",
      "Implica la obligación de cotizar en el régimen de trabajadores autónomos o, alternativamente, en la Mutualidad de la Abogacía.",
      "Implica la renuncia a prestar servicios como abogado en una sociedad sujeta al régimen de sociedad profesionales.",
      "No está sujeto a normativa alguna."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "Javier, abogado colegiado, gestiona desde hace unos años una serie de fideicomisos de los que se beneficia Enric y una serie de sociedades vinculadas con este último. Javier:",
    options: [
      "Está obligado a reportar al SEPBLAC cualquier indicio de blanqueo de capitales.",
      "Está obligado a llevar a cabo un seguimiento continuo de la relación de negocios con Enric y las sociedades vinculadas.",
      "Todas las respuestas restantes son correctas.",
      "Está obligado a dejar de gestionar los fideicomisos ante cualquier indicio de blanqueo de capitales."
    ],
    correct: 2
  },
  {
    section: "Examen Final 2023",
    text: "Rut, tras haber sido detenida por la policía y puesta en libertad con cargos, le confiesa a Pau, abogado, que desde hace un tiempo venía participando en la gestión de un entramado societario dedicado al blanqueo de capitales. Rut le pide a Pau que asuma su defensa penal. Pau:",
    options: [
      "Debe informar al Servicio Ejecutivo de la Comisión de Prevención de Blanqueo de Capitales e Infracciones Monetarias del delito de blanqueo de capitales.",
      "Puede asumir la defensa penal de Rut sin pasar por ello a ser sujeto obligado por la Ley 10/2010.",
      "Debe abstenerse de llevar el asunto dado que Rut es responsable confesa de un delito de blanqueo de capitales.",
      "En tanto que abogado defensor de Rut, está sometido a todas las obligaciones de la Ley 10/2010, aunque puede abstenerse de comunicar el indicio de blanqueo."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "¿En el marco de las relaciones laborales especiales de los abogados es posible celebrar un pacto de no competencia postcontractual?",
    options: [
      "No.",
      "Sí, durante un periodo máximo de 2 años y el ámbito de prohibición puede alcanzar al ejercicio de la profesión en su totalidad.",
      "Sí, durante un periodo máximo de dos años, si bien el ámbito de la prohibición solo puede referirse a clientes del despacho o asuntos en los que haya intervenido el abogado.",
      "Sí, durante un periodo máximo de dos años y el ámbito de la prohibición puede referirse a la especialidad del abogado, pero no al ejercicio de la profesión."
    ],
    correct: 2
  },
  {
    section: "Examen Final 2023",
    text: "Cuando un procurador recibe una notificación del juzgado, la fecha de notificación es:",
    options: [
      "El día en que el procurador se la envía al abogado (Art. 151.2 LEC).",
      "Si la recepción es por vía telemática, la notificación es el día de la recepción (Art.151.3 LEC).",
      "El día en que se firmó la resolución por el juez (Art.151.1 LEC).",
      "Al día siguiente de su recepción por el Colegio de Procuradores (Art.151.2 LEC)."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "Indique cuál de las siguientes frases es correcta (retención en factura de servicios jurídicos):",
    options: [
      "Existe obligación de practicar retención en el pago de una factura de servicios jurídicos de asesoramiento, siempre que el destinatario de la factura sea un particular (una persona física NO titular de una actividad económica).",
      "Existe obligación de practicar retención en el pago de una factura de servicios jurídicos de asesoramiento, siempre que el destinatario de la factura sea una persona física que realiza el pago en ejercicio de su actividad económica.",
      "Ninguna es correcta.",
      "En ningún caso existe obligación de practicar retención en el pago de una factura de servicios jurídicos de asesoramiento."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "Araceli tiene varias cuentas corrientes en un banco de Panamá y le pide a Miguel, abogado colegiado, que realice las gestiones necesarias para traer todo el dinero a España, manifestándole que el dinero proviene de la venta de una casa en Panamá. Miguel:",
    options: [
      "Debe extremar la cautela y, en caso de aceptar el encargo, hacer primeramente un examen especial.",
      "En caso de aceptar el encargo, debe informar en primer lugar al SEPBLAC por ser una operación sospechosa.",
      "Debe abstenerse de asumir el encargo dado que los fondos se encuentran en un paraíso fiscal.",
      "En cumplimiento de la Ley 10/2010, debe poner en conocimiento de la Autoridades Fiscales españolas la existencia de las cuentas corrientes en Panamá."
    ],
    correct: 0
  },
  {
    section: "Examen Final 2023",
    text: "Si el abogado recibe con cargo al despacho en el que presta servicios una formación o especialización profesional:",
    options: [
      "No puede suscribirse un pacto de permanencia, pero el abogado solo podrá prestar en exclusiva sus servicios al despacho que ha sufragado el coste de su formación.",
      "Puede suscribirse un pacto de permanencia de una duración máxima de 1 año.",
      "No puede suscribirse un pacto de permanencia.",
      "Puede suscribirse un pacto de permanencia de una duración máxima de 2 años."
    ],
    correct: 3
  },
  {
    section: "Examen Final 2023",
    text: "Los sujetos obligados del Art. 31 del Reglamento no están exceptuados (= están obligados) de cumplir y documentar una de las obligaciones que se indican a continuación. Indicar cuál:",
    options: [
      "Aprobar y aplicar un plan de formación de empleados.",
      "Documentar una política de admisión de clientes.",
      "Elaborar un Manual de Prevención del Blanqueo de capitales.",
      "Documentar la identificación formal de sus clientes."
    ],
    correct: 3
  },
  {
    section: "Examen Final 2023",
    text: "¿Qué ocurre si el destinatario de un acto de comunicación (un emplazamiento) se identifica pero se niega a recoger la documentación?",
    options: [
      "Depende de si la casa tiene conserje.",
      "Hay que llamar a la Policía.",
      "Se le notificará por edictos en el tablón de anuncios del juzgado y se le declarará en rebeldía.",
      "Se dejará constancia de ello en la diligencia y se apercibirá al destinatario que el acto tendrá efectos de comunicación, que los plazos empezarán a contar y que tendrá la documentación a su disposición en el juzgado."
    ],
    correct: 3
  },
  {
    section: "Examen Final 2023",
    text: "El Sr. X obtuvo la habilitación para ejercer como abogado en fecha 1/4/2018 siendo contratado el mismo día por una empresa privada. Su contrato con la empresa privada finalizó el 1/6/2020 y comienza a prestar servicios –sin tener la condición de socio– para un despacho de abogados:",
    options: [
      "El despacho debe contratar a este abogado con un contrato de trabajo ordinario sometido al Estatuto de los Trabajadores.",
      "El despacho debe contratar a este abogado con un contrato de trabajo sujeto al RD 1331/2006 de 27 de noviembre y puede utilizar la modalidad de contrato en prácticas.",
      "Ninguna de las restantes respuestas es correcta.",
      "El despacho debe contratar a este abogado con un contrato de trabajo sujeto al RD 1331/2006 de 27 de noviembre, pero no puede utilizar la modalidad de contrato en prácticas."
    ],
    correct: 3
  },
  {
    section: "Examen Final 2023",
    text: "Tenemos que interponer una demanda en Logroño, pero el procurador que aparece en el poder se ha jubilado. ¿Qué podemos hacer?",
    options: [
      "No hace falta hacer nada. El Colegio de Procuradores de Logroño designará un sustituto automáticamente al presentar la demanda.",
      "Si el cliente no lo ha prohibido, podemos pedir a otros procuradores (que consten en el poder) que sustituyan dicho poder en favor de un procurador de Logroño que esté en activo.",
      "Depende del tipo de procedimiento de que se trate. Si no es preceptivo procurador, tampoco es necesario acreditar la representación.",
      "No hace falta hacer nada porque un procurador o su oficial habilitado pueden sustituir a otro procurador, aunque esté jubilado y todavía no se haya personado en las actuaciones (Art. 543.4 LOPJ)."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "La Diligencia Debida que se exige al Sujeto Obligado contempla una de las siguientes obligaciones:",
    options: [
      "La afirmación de la pregunta es falsa: no contempla ninguna de las restantes.",
      "Hacer un seguimiento continuo de la relación con los clientes.",
      "Realizar un examen especial.",
      "Formar al personal para que detecten los riesgos."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "Tres abogados constituyen una sociedad civil particular para el ejercicio de su actividad profesional. En el primer ejercicio obtienen un beneficio de 10.000 €. Indicar cómo tributará el resultado económico de esta actividad:",
    options: [
      "Tributará en el impuesto sobre la renta del socio de más edad, porque el objeto de la sociedad es el ejercicio de una actividad profesional.",
      "Tributará en el Impuesto sobre el Patrimonio de la sociedad.",
      "Ninguna respuesta es correcta.",
      "Tributará en el Impuesto sobre Sociedades, puesto que es una sociedad."
    ],
    correct: 2
  },
  {
    section: "Examen Final 2023",
    text: "El responsable del tratamiento de los datos es:",
    options: [
      "Aquella persona física o jurídica que, solo o conjuntamente con otros, determina las finalidades y medios del tratamiento.",
      "La persona física o jurídica que trata los datos personales por encargo del responsable del tratamiento.",
      "El interesado que comparte sus datos previo consentimiento.",
      "La persona física o jurídica, autoridad pública, Servicio u otro organismo que, solo o junto con otros, determina los fines y medios del tratamiento."
    ],
    correct: 3
  },
  {
    section: "Examen Final 2023",
    text: "De acuerdo con lo dispuesto en el Reglamento General Europeo de Protección de Datos, ¿quién es la persona afectada o interesada?",
    options: [
      "La persona física titular de los datos que sean objeto del tratamiento.",
      "El titular de la administración pública que gestiona los datos.",
      "Ninguna es correcta.",
      "El titular del registro de actividades de protección de datos."
    ],
    correct: 0
  },
  {
    section: "Examen Final 2023",
    text: "Conforme a la Ley 10/2010 el abogado debe comunicar al SEPBLAC cuando:",
    options: [
      "Un cliente que posee una cuenta bancaria oculta en un paraíso fiscal le pide asesoramiento legal sobre las consecuencias y los pasos que debería dar para regularizar el depósito en España.",
      "Durante el procedimiento de divorcio de su cliente, alcalde de un municipio de más de 50.000 habitantes, aflora que este tiene una empresa de construcción a través de testaferros vinculados a su exesposa, a la que se han adjudicado contratos «a dedo» en su municipio.",
      "Asesorando a un cliente promotor inmobiliario descubre que una de sus socias, esposa del Regidor de Urbanismo de la Capital de la Comunidad Autónoma, ha cobrado en concepto de dividendos el doble que el resto de los socios.",
      "Un cliente investigado por narcotráfico le da el nombre de los testaferros que ocultan sus bienes y a quienes está tratando de identificar la Policía Judicial."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "En relación con la tributación de los servicios jurídicos en el IVA, indicar cuál de las respuestas es correcta:",
    options: [
      "Ninguna de las demás respuestas es correcta.",
      "Los servicios jurídicos están sujetos a tributación en el IVA al tipo reducido del 10%.",
      "Los servicios jurídicos están sujetos a tributación en el IVA al tipo general del 21%, únicamente en caso de que el importe de la factura sea superior a 900,00 €.",
      "Los servicios jurídicos están sujetos pero exentos de tributación en el IVA."
    ],
    correct: 0
  },
  {
    section: "Examen Final 2023",
    text: "¿En qué consiste el consentimiento del interesado (RGPD)?",
    options: [
      "La manifestación del titular del Registro de Actividades del tratamiento a ceder los datos al interesado.",
      "Es toda manifestación de voluntad, libre, inequívoca, específica e informada mediante la cual el interesado consiente al tratamiento de sus datos personales.",
      "En la no oposición al tratamiento de sus datos.",
      "En la manifestación del responsable de protección de datos de hacer difusión de los mismos."
    ],
    correct: 1
  },
  {
    section: "Examen Final 2023",
    text: "La promoción a socio del despacho profesional del abogado con contrato sujeto al RD 1331/2006:",
    options: [
      "Permite al abogado trabajar en otro despacho sin restricción alguna.",
      "No tiene incidencia alguna sobre el régimen laboral al que está sujeto el abogado.",
      "Supone la extinción inmediata del contrato sujeto al RD 1331/2006.",
      "El contrato laboral sujeto al RD se mantiene en suspenso durante 2 años."
    ],
    correct: 2
  },
  {
    section: "Examen Final 2023",
    text: "Los abogados contratados al amparo del RD 1331/2006:",
    options: [
      "Además de las obligaciones propias de la normativa laboral, están sujetos a las normas deontológicas, estatutarias y éticas que rigen la profesión.",
      "Se rigen exclusivamente por el principio de independencia.",
      "No deben estar habilitados para el ejercicio de la profesión.",
      "Tienen las mismas obligaciones laborales que los trabajadores comunes."
    ],
    correct: 0
  },
  // ───── EXAMEN 2019 ─────
  {
    section: "Examen 2019",
    text: "La comparecencia en juicio será por medio de procurador, no obstante los litigantes podrán comparecer por sí mismos:",
    options: [
      "En los procesos de impugnación de acuerdos sociales tramitados ante los Juzgados de lo Mercantil.",
      "En los juicios verbales cuya determinación se haya efectuado en razón de la cuantía y ésta no exceda de 2.000 € y para la petición inicial de los procedimientos monitorios.",
      "En los juicios ordinarios tramitados por razón de la materia.",
      "Para la oposición al monitorio, sea cual sea la cuantía objeto de la reclamación."
    ],
    correct: 1
  },
  {
    section: "Examen 2019",
    text: "El procurador, una vez aceptado el poder, entre otras obligaciones, debe:",
    options: [
      "Pagar todos los gastos del proceso, incluidas las costas a cuyo pago pueda ser condenado su representado.",
      "Acudir semanalmente a las salas de notificaciones y servicios comunes para recoger las resoluciones de los Juzgados.",
      "Trasladar los escritos de su poderdante y de su Letrado a los Procuradores de las restantes partes en la forma prevista en el art. 276 de la LEC.",
      "Trasladar a su poderdante y al abogado únicamente las copias de aquellas resoluciones que él considere importantes para el normal transcurso del procedimiento."
    ],
    correct: 2
  },
  {
    section: "Examen 2019",
    text: "Los actos de comunicación se realizarán bajo la dirección del Letrado de la Administración de Justicia. Dichos actos se ejecutarán:",
    options: [
      "Por el abogado que firma la demanda.",
      "Conjuntamente por el Magistrado y el Letrado de la Administración de Justicia.",
      "Únicamente podrán ser realizados por los funcionarios de Justicia.",
      "Podrán ejecutarse con los mismos efectos, tanto por los funcionarios del Cuerpo de Auxilio Judicial como por el procurador de la parte que así lo solicite."
    ],
    correct: 3
  },
  {
    section: "Examen 2019",
    text: "¿Cuál es el tipo de IVA aplicable a los servicios jurídicos?",
    options: [
      "El tipo de IVA aplicable a los servicios jurídicos es el tipo reducido.",
      "El tipo de IVA aplicable a los servicios jurídicos es el tipo general, salvo los prestados con ocasión del Servicio del Turno de Oficio y Asistencia al Detenido, que tributan a tipo reducido.",
      "El tipo de IVA aplicable a los servicios jurídicos es el tipo general.",
      "El tipo de IVA aplicable a los servicios jurídicos es el tipo superreducido."
    ],
    correct: 2
  },
  {
    section: "Examen 2019",
    text: "En el caso de actividades profesionales ejercidas por una persona física, y en relación con la obligación de retener, indicar cuál de estas afirmaciones es correcta:",
    options: [
      "En caso de que el pago esté sujeto a retención, el sujeto que está obligado a practicar retención y responsable del ingreso del importe retenido es el pagador del rendimiento.",
      "Ningún rendimiento derivado de una actividad profesional está sujeto a retención a cuenta del Impuesto sobre la Renta de las Personas Físicas.",
      "Por imperativo de la normativa tributaria, la sociedad receptora es la obligada a retener y la obligada a realizar el ingreso.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 0
  },
  {
    section: "Examen 2019",
    text: "En el caso de actividades profesionales ejercidas por una persona física, indicar cuál es el régimen de estimación aplicable para determinar el rendimiento neto de la actividad en el IRPF:",
    options: [
      "El régimen de estimación objetiva singular complicada.",
      "El régimen de estimación objetiva singular simplificada.",
      "El régimen de estimación global de bases.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 3
  },
  {
    section: "Examen 2019",
    text: "¿Cuál de las siguientes actividades profesionales está excluida de la aplicación de la Ley de prevención del blanqueo de capitales?",
    options: [
      "El asesoramiento para constituir sociedades mercantiles.",
      "El asesoramiento para invertir en la adquisición de un inmueble.",
      "El asesoramiento para evitar una demanda judicial para el incumplimiento del contrato de adquisición de un inmueble.",
      "El asesoramiento para abrir una peluquería en la ciudad de Barcelona."
    ],
    correct: 2
  },
  {
    section: "Examen 2019",
    text: "¿Cuál de las siguientes obligaciones del abogado no está comprendida dentro de los procedimientos de diligencia debida?",
    options: [
      "La obligación de identificar al titular real del cliente.",
      "La identificación formal del cliente.",
      "El seguimiento continuo de la relación de negocio.",
      "La colaboración con el servicio ejecutivo de la Comisión de la prevención de blanqueo (SEPBLAC)."
    ],
    correct: 3
  },
  {
    section: "Examen 2019",
    text: "El Manual de prevención del blanqueo debe incorporar:",
    options: [
      "Una política de admisión de clientes donde se identificarán los clientes con riesgo de blanqueo superior a la media con las medidas que deben adoptar para mitigar este riesgo.",
      "Un procedimiento estructurado de aplicación de la Diligencia Debida.",
      "Un procedimiento estructurado de examen especial.",
      "Todas las anteriores."
    ],
    correct: 3
  },
  {
    section: "Examen 2019",
    text: "El Reglamento General Europeo de protección de Datos, ¿a qué empresas u organizaciones se aplica?",
    options: [
      "A responsables o encargados de tratamiento de datos establecidos en la Unión Europea.",
      "A responsables y encargados no establecidos en la UE siempre que realicen tratamientos derivados de una oferta de bienes o servicios destinados a ciudadanos de la Unión o como consecuencia de una monitorización y seguimiento de su comportamiento.",
      "Las dos primeras son correctas.",
      "Sólo se aplica a los ficheros declarados ante la Agencia Española de Protección de Datos."
    ],
    correct: 2
  },
  {
    section: "Examen 2019",
    text: "Según el RGPD ¿Cómo debe solicitarse el consentimiento de los interesados para tratar sus datos personales?",
    options: [
      "El RGPD requiere que el consentimiento sea «inequívoco», lo que supone que se preste mediante una manifestación del interesado o mediante una clara acción afirmativa.",
      "El RGPD permite el consentimiento tácito para datos exclusivamente identificativos.",
      "El consentimiento no es un elemento determinante para el RGPD.",
      "El RGPD requiere el consentimiento, independientemente de que sea expreso o tácito, si se desprende del contenido del acto jurídico."
    ],
    correct: 0
  },
  {
    section: "Examen 2019",
    text: "El derecho al olvido se debe ejercer:",
    options: [
      "Ante el Responsable del Tratamiento.",
      "Ante un Notario.",
      "Ante los Tribunales de la jurisdicción civil.",
      "Ante las empresas responsables de motores de búsqueda en Internet."
    ],
    correct: 0
  },
  {
    section: "Examen 2019",
    text: "El Contrato de trabajo indefinido que articula una relación laboral especial de un abogado que presta servicios en un despacho individual o colectivo puede tener un periodo de prueba máximo de:",
    options: ["2 meses.", "1 mes.", "3 meses.", "6 meses."],
    correct: 3
  },
  {
    section: "Examen 2019",
    text: "La relación laboral especial de un abogado que presta sus servicios en un despacho individual o colectivo es posible canalizarla a través de un contrato en prácticas:",
    options: [
      "Siempre y en todo caso sin ningún tipo de limitación.",
      "Solo dentro de los 5 años siguientes a la obtención del título que habilite para ejercer la profesión.",
      "Solo dentro de los 3 años siguientes a la obtención del título que habilite para ejercer la profesión.",
      "Sólo en el caso de que el abogado no hubiera estado vinculado con otro despacho con otro contrato en prácticas."
    ],
    correct: 1
  },
  {
    section: "Examen 2019",
    text: "En el marco de una relación laboral especial de abogado que presta servicios en despacho individual o colectivo, la retribución recogida en el contrato:",
    options: [
      "Sólo es debida en el caso de que los clientes para los que haya trabajado el abogado hayan pagado los honorarios del despacho.",
      "Sólo es debida en el caso de que la cuenta de resultados del despacho sea positiva.",
      "Siempre y en todo caso es debida.",
      "Es debida siempre que no exista una quiebra de la confianza entre el abogado y el titular del despacho."
    ],
    correct: 2
  },
  {
    section: "Examen 2019 — Reserva",
    text: "En las relaciones entre el Procurador y su poderdante, a falta de disposición expresa, regirán:",
    options: [
      "Las normas de conducta que fije para cada caso el Tribunal.",
      "Las pautas que haya pactado previamente con el letrado director del asunto.",
      "Dichas relaciones se regirán según las normas establecidas para el contrato de mandato en la legislación civil aplicable.",
      "Dependerá de la jurisdicción en la que se actúe."
    ],
    correct: 2
  },
  {
    section: "Examen 2019 — Reserva",
    text: "En caso de ejercicio de una actividad profesional de prestación de servicios jurídicos por parte de una persona física, indicar en qué supuesto existe obligación de presentar declaración trimestral de pago fraccionado a cuenta del IRPF:",
    options: [
      "En todo caso.",
      "En ningún caso.",
      "No existe obligación de realizar pagos fraccionados si en el año natural anterior al menos el 70% de los ingresos de la actividad fueron objeto de retención o ingreso a cuenta.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 2
  },
  {
    section: "Examen 2019 — Reserva",
    text: "La información que damos a un cliente sobre el tratamiento de sus datos (cláusula informativa):",
    options: [
      "Se presenta con la demanda ante los Tribunales.",
      "Se recomienda incluirla junto con la documentación de encargo del servicio, al inicio de la relación profesional.",
      "Solamente tiene efectos en el ámbito civil.",
      "No es de aplicación en la relación abogado-cliente."
    ],
    correct: 1
  },
  // ───── EXAMEN 2018 ─────
  {
    section: "Examen 2018",
    text: "Los socios de un despacho colectivo de abogados que adopta la forma de sociedad profesional tienen con dicha sociedad:",
    options: [
      "Una relación laboral ordinaria.",
      "Una relación laboral especial.",
      "Una relación societaria de carácter mercantil.",
      "Ninguna de las contestaciones anteriores es cierta."
    ],
    correct: 2
  },
  {
    section: "Examen 2018",
    text: "El abogado contratado por una empresa privada para responsabilizarse de su departamento legal que se encuentra sujeto al horario establecido por la empresa, la cual le proporciona a su vez todos los medios necesarios para realizar su trabajo:",
    options: [
      "Mantiene una relación laboral ordinaria con la empresa.",
      "Mantiene con esa empresa una relación laboral especial sujeta al RD 1331/2006, 17 de noviembre.",
      "Mantiene una relación civil de prestación de servicios profesionales.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 0
  },
  {
    section: "Examen 2018",
    text: "La jornada anual de un abogado con relación laboral especial:",
    options: [
      "Puede ser superior a la prevista en el Estatuto de los Trabajadores.",
      "No puede ser superior a la prevista en el Estatuto de los Trabajadores.",
      "En las relaciones laborales especiales de los abogados con los despachos profesionales no resultan aplicables las previsiones sobre jornada anual previstas en el Estatuto de los trabajadores.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 1
  },
  {
    section: "Examen 2018",
    text: "El principio de calidad de los datos previsto en la Ley Orgánica 15/1999 de protección de datos dispone:",
    options: [
      "Que los datos recogidos deben ser adecuados, pertinentes y tan amplios como sea posible en relación al ámbito y las finalidades determinadas, explícitas y legítimas para las que se han obtenido.",
      "Que los datos recogidos deben ser adecuados, pertinentes y no excesivos en relación al ámbito y las finalidades determinadas, explícitas y legítimas para las que se han obtenido.",
      "Que los datos recogidos deben ser los mejores para conseguir la finalidad buscada.",
      "Que los datos recogidos deben ser todos los que nos quiera proporcionar el interesado."
    ],
    correct: 1
  },
  {
    section: "Examen 2018",
    text: "El derecho al olvido implica:",
    options: [
      "Que los interesados pueden solicitar a los medios de información que borren las noticias en las que aparecen sus datos personales.",
      "Que los interesados pueden solicitar a los buscadores que modifiquen sus motores de búsqueda para evitar que encuentren determinadas referencias, siempre y cuando se den los requisitos necesarios.",
      "Que los tribunales eliminarán de la prensa aquellas noticias que hagan referencia a los interesados.",
      "Que los interesados tienen derecho a modificar su currículum digital de la forma que más les convenga."
    ],
    correct: 1
  },
  {
    section: "Examen 2018",
    text: "El consentimiento del afectado, tal como dispone la LOPD:",
    options: [
      "Debe ser inequívoco.",
      "No es necesario cuando el tratamiento de sus datos tenga como finalidad proteger un interés vital del interesado en los términos previstos en el art. 7.6 de la LOPD.",
      "No es necesario cuando los datos figuren en fuentes accesibles al público.",
      "Todas las anteriores."
    ],
    correct: 3
  },
  {
    section: "Examen 2018",
    text: "La comparecencia en juicio será por medio de Procurador, no obstante los litigantes podrán comparecer por sí mismos (Examen 2018):",
    options: [
      "En los juicios verbales cuya determinación se haya efectuado en razón de la cuantía y ésta no exceda de 2.000 €.",
      "En los procesos en los que sólo sea preceptiva la intervención del letrado.",
      "En los juicios ordinarios tramitados por razón de la materia.",
      "En todos los juicios universales, sea cual sea la actuación a desarrollar dentro de dichos procesos."
    ],
    correct: 0
  },
  {
    section: "Examen 2018",
    text: "El procurador, una vez aceptado el poder, entre otras obligaciones, debe (Examen 2018):",
    options: [
      "Pagar todos los gastos del proceso, incluidas las costas a cuyo pago pueda ser condenado su representado.",
      "Pagar todos los gastos causados a su instancia, excepto los honorarios de los abogados y los correspondientes a los peritos, las tasas por el ejercicio de la potestad jurisdiccional y los depósitos necesarios para la presentación de recursos, a no ser que el poderdante le haya dado fondos para el pago de los mismos.",
      "Sólo deberá pagar los gastos en los procedimientos tramitados ante los Juzgados de 1ª Instancia en los que haya condena en costas a su representado.",
      "No debe pagar gasto alguno."
    ],
    correct: 1
  },
  {
    section: "Examen 2018",
    text: "Los actos de comunicación se realizarán bajo la dirección del Letrado de la Administración de Justicia. Dichos actos se ejecutarán (Examen 2018):",
    options: [
      "Por el abogado que firma la demanda.",
      "Conjuntamente por el Magistrado y el Letrado de la Administración de Justicia.",
      "Únicamente podrán ser realizados por los funcionarios de Justicia.",
      "Podrán ejecutarse con los mismos efectos, tanto por los funcionarios del Cuerpo de Auxilio Judicial como por el procurador de la parte que así lo solicite."
    ],
    correct: 3
  },
  {
    section: "Examen 2018",
    text: "En caso de ejercicio de la actividad profesional mediante una sociedad civil particular profesional, ¿en qué tributo estará sujeto a tributación el beneficio obtenido en el ejercicio de la actividad?",
    options: [
      "Una sociedad civil privada no puede ser titular de una actividad empresarial o profesional debido a que no tiene personalidad jurídica.",
      "El beneficio empresarial obtenido por una sociedad civil estará sujeto a tributación en el Impuesto sobre Sociedades, puesto que el hecho imponible de este impuesto es la obtención de rentas por parte de personas jurídicas.",
      "El beneficio empresarial estará sujeto a tributación en el Impuesto sobre la Renta de las Personas Físicas de los socios, en régimen de atribución de rentas, puesto que el hecho imponible de este impuesto es la obtención de rentas por parte de personas físicas, incluyendo las percibidas por atribución.",
      "El beneficio empresarial estará sujeto a tributación en el Impuesto sobre el Valor Añadido, puesto que el hecho imponible de este impuesto es la obtención de beneficios derivados de la prestación de servicios."
    ],
    correct: 2
  },
  {
    section: "Examen 2018",
    text: "En el caso de actividades profesionales ejercidas por una persona física, y en relación con la obligación de retener, indicar cuál es la respuesta correcta (Examen 2018):",
    options: [
      "Por imperativo de la normativa tributaria, debe practicarse retención en todos los pagos que se realicen como contraprestación de servicios profesionales.",
      "Ningún rendimiento derivado de una actividad profesional está sujeto a retención a cuenta del Impuesto sobre la Renta de las Personas Físicas.",
      "Los rendimientos de las actividades profesionales están sujetas a retención cuando el pagador del rendimiento es una persona física.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 3
  },
  {
    section: "Examen 2018",
    text: "En el Impuesto sobre el Valor Añadido, el tipo impositivo aplicable a los servicios jurídicos es:",
    options: [
      "El tipo superreducido del 3% en el caso de servicios prestados en el turno de oficio y asistencia al detenido.",
      "El tipo reducido del 7% en el caso de servicios de litigación penal prestados a personas con riesgo de exclusión social.",
      "El tipo general del 21%.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 2
  },
  {
    section: "Examen 2018",
    text: "¿Cuál de las cuatro obligaciones de los sujetos obligados que se recogen a continuación no se corresponde con las obligaciones de diligencia debida?",
    options: [
      "Identificación del titular real de la operación.",
      "Identificación formal.",
      "Deber de comunicación.",
      "Seguimiento continuo de la relación de negocio."
    ],
    correct: 2
  },
  {
    section: "Examen 2018",
    text: "¿Cuál de las siguientes sujetos es una persona con responsabilidad pública?",
    options: [
      "El Consejero Delegado del BBVA.",
      "El Presidente del FC Barcelona.",
      "El Regidor de urbanismo del Ayuntamiento de Barcelona.",
      "Ninguno de los tres."
    ],
    correct: 2
  },
  {
    section: "Examen 2018",
    text: "¿Cuál de las siguientes afirmaciones es correcta (operación sospechosa)?",
    options: [
      "Si detectamos una operación sospechosa es prioritario evitar que el cliente sepa que vamos a realizar una comunicación al SEPBLAC.",
      "Si detectamos una operación sospechosa siempre debemos cesar inmediatamente la relación con el cliente.",
      "Ninguna de las dos afirmaciones es correcta.",
      "Las dos afirmaciones son correctas."
    ],
    correct: 0
  },
  {
    section: "Examen 2018 — Reserva",
    text: "En las relaciones entre el Procurador y su poderdante, a falta de disposición expresa, regirán (Examen 2018):",
    options: [
      "Las normas de conducta que fije para cada caso el Tribunal.",
      "Las pautas que haya pactado previamente con el letrado director del asunto.",
      "Dichas relaciones se regirán según las normas establecidas para el contrato de mandato en la legislación civil aplicable.",
      "Dependerá de la jurisdicción."
    ],
    correct: 2
  },
  // ───── DEONTOLOGÍA ─────
  {
    section: "Deontología — Honorarios",
    text: "Si se impugna por indebida la minuta de honorarios de la defensa letrada incluida en una tasación de costas:",
    options: [
      "El LAJ debe remitir las actuaciones al Colegio de Abogados para que emita el informe preceptivo que le vincula en su decisión.",
      "El LAJ debe remitir las actuaciones al Colegio de Abogados para que emita el informe preceptivo sin que éste le vincule en su decisión.",
      "El LAJ debe resolver sin más trámite mediante Decreto declarando indebida la minuta impugnada.",
      "El LAJ debe dar traslado por tres días a la defensa letrada minutante y seguidamente resolver mediante Decreto."
    ],
    correct: 3
  },
  {
    section: "Deontología — Honorarios",
    text: "Si la impugnación por excesivos de los honorarios de la defensa letrada incluidos en una tasación es total o parcialmente estimada:",
    options: [
      "No habrá condena en costas del incidente.",
      "Se impondrán las costas al abogado cuyos honorarios se hubieren considerado excesivos.",
      "Sólo se impondrán las costas si se aprecia temeridad o mala fe en la defensa letrada minutante.",
      "Sólo se impondrán las costas si el informe del Colegio de Abogados se pronuncia en el mismo sentido."
    ],
    correct: 0
  },
  {
    section: "Deontología — Honorarios",
    text: "En un presupuesto de honorarios al propio cliente, el importe:",
    options: [
      "Se fijará siempre en una cuota litis sobre el resultado que se obtenga.",
      "Se fijará en todos los supuestos en función de lo que resulte de la aplicación de los Criterios Orientadores del Colegio de Abogados al que pertenezca el profesional de la abogacía.",
      "Será el que libremente pacten el profesional de la abogacía y su cliente, con respeto a las normas deontológicas y sobre competencia.",
      "Se fijará siempre en función de las horas aproximadas de dedicación."
    ],
    correct: 2
  },
  {
    section: "Deontología — Honorarios",
    text: "Es competente para conocer del procedimiento de jura de cuentas en reclamación de honorarios:",
    options: [
      "Siempre el juzgado de primera instancia del domicilio del deudor.",
      "El Juzgado o Tribunal que haya conocido del procedimiento o actuación judicial del cual deriven los honorarios reclamados.",
      "Siempre el juzgado de primera instancia del domicilio de la defensa letrada minutante.",
      "El juzgado de primera instancia del domicilio de la defensa letrada minutante o del deudor, a elección del demandante."
    ],
    correct: 1
  },
  {
    section: "Deontología — Honorarios",
    text: "Contra el Decreto que resuelva el incidente de impugnación por excesivos de honorarios reclamados a través del procedimiento de jura de cuentas:",
    options: [
      "No cabe recurso.",
      "Cabe recurso de revisión.",
      "Cabe recurso de reposición.",
      "Cabe recurso de queja."
    ],
    correct: 0
  },
  {
    section: "Deontología — Honorarios",
    text: "Si en un procedimiento ordinario en reclamación de honorarios el demandado se opone:",
    options: [
      "Siempre se dará traslado al Colegio de Abogados para que emita el dictamen del artículo 246.1 LEC.",
      "Se dará traslado al Colegio de Abogados para que emita el dictamen del artículo 246.1 LEC, si así lo solicita cualquiera de las partes.",
      "Se dará traslado al Colegio de Abogados para que emita el dictamen del artículo 246.1 LEC si el juez lo considera oportuno.",
      "Nunca se dará traslado al Colegio de Abogados para que emita el dictamen del artículo 246.1 LEC, si bien cualquiera de las partes podrá solicitar que se designe un perito experto en la materia."
    ],
    correct: 1
  },
  {
    section: "Deontología — Honorarios",
    text: "El cliente no ha pagado la minuta y el letrado decide instar la reclamación de honorarios del artículo 35 LEC. ¿Ante qué juzgado debe hacerlo?",
    options: [
      "El del domicilio del demandado.",
      "El pactado entre las partes.",
      "El del domicilio del letrado.",
      "Ante el mismo Juzgado en el que se ha tramitado el procedimiento principal."
    ],
    correct: 3
  },
  {
    section: "Deontología — Honorarios",
    text: "En la tasación de costas, ¿puede impugnarse la minuta del Letrado simultáneamente por excesiva y por indebida?",
    options: [
      "En ningún caso. La parte debe optar necesariamente entre impugnar los honorarios por excesivos o por indebidos.",
      "Sí, en cuyo caso se tramitan ambas impugnaciones simultáneamente con arreglo a sus propias normas, pero la resolución sobre si los honorarios son excesivos quedará en suspenso hasta que se decida sobre si la partida impugnada es o no debida.",
      "No. Se podrán impugnar por excesivos, haciendo expresa reserva en el mismo escrito de impugnación de que para el caso de no prosperar esta impugnación por excesivos se dejan impugnados por indebidos con carácter subsidiario.",
      "Se pueden impugnar los honorarios por excesivos e indebidos de forma simultánea, quedando en suspenso la resolución por indebidos hasta que no se emita informe por parte del Colegio de Abogados."
    ],
    correct: 1
  },
  {
    section: "Deontología — Honorarios",
    text: "Una minuta impugnada por excesiva en la jura de cuentas (art. 35 LEC): ¿qué criterios se tendrán en cuenta en la emisión del dictamen colegial?",
    options: [
      "Los Criterios Orientadores en materia de honorarios profesionales aprobados por la Junta de Gobierno en fecha 21 de diciembre de 2009.",
      "Las Pautas Básicas en materia de Criterios Orientadores de honorarios profesionales, aprobadas por Acuerdo de la Junta de Gobierno de 20 de marzo de 2018 (Derogadas).",
      "Cualquiera de las dos normativas previstas en los apartados a) y b).",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 3
  },
  {
    section: "Deontología — Honorarios",
    text: "Marta, abogada de la parte demandada en un juicio verbal, tiene que presentar su minuta de honorarios para incluir en la tasación de costas del procedimiento y no ha suscrito ningún pacto con su cliente. De acuerdo con las Pautas Básicas del ICAB, ¿cómo fijará su importe?",
    options: [
      "En la tercera parte de la cuantía del procedimiento, de conformidad con el artículo 394.3 de la LEC.",
      "En el importe que considere razonable.",
      "Ponderando de forma conjunta el resultado del procedimiento, la cuantía procesal o interés económico real, el tipo del procedimiento, el tiempo dedicado y el grado de dificultad.",
      "Si no ha suscrito un pacto con su cliente no puede presentar minuta de honorarios."
    ],
    correct: 2
  },
  {
    section: "Deontología — Honorarios",
    text: "Si interponemos un procedimiento monitorio en reclamación de honorarios profesionales y la parte demandada ni atiende al requerimiento de pago ni comparece, ¿cómo continuará su tramitación?",
    options: [
      "Se dictará decreto dando por terminado el proceso monitorio y dando traslado al acreedor para que inste el despacho de ejecución, bastando para ello con la mera solicitud, sin necesidad de que transcurra el plazo de 20 días previsto en el artículo 548 de la LEC.",
      "Se archivará el procedimiento, sin más trámite.",
      "Se despachará directamente ejecución por la cantidad a que ascienda la minuta.",
      "Se dictará decreto acordando su continuación por los trámites del juicio verbal u ordinario en función de la cantidad reclamada."
    ],
    correct: 0
  },
  {
    section: "Deontología — Honorarios",
    text: "En un juicio verbal se dicta sentencia con condena en costas a favor de una pluralidad de litigantes que han comparecido con diferente dirección letrada. ¿Cómo se aplicará el límite de la tercera parte de la cuantía del proceso previsto en el artículo 394.3 de la LEC?",
    options: [
      "En este caso no se aplicará dicho límite.",
      "El litigante vencido estará obligado a pagar, de la parte que corresponda a los abogados y demás profesionales que no estén sujetos a tarifa o arancel, una cantidad total que no exceda de la tercera parte de la cuantía del proceso, por cada uno de los litigantes favorecidos en costas.",
      "El litigante vencido estará obligado a pagar, por todos los conceptos, una cantidad total que no exceda de la tercera parte de la cuantía del proceso, con independencia del número de litigantes favorecidos en costas.",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 1
  },
  {
    section: "Deontología — Honorarios",
    text: "Si un cliente nos encarga su defensa como parte actora en un procedimiento ordinario sobre reclamación de cantidad y al finalizar el encargo profesional no nos paga los honorarios pactados, podremos reclamárselos judicialmente interponiendo:",
    options: [
      "Un incidente de tasación de costas.",
      "Un procedimiento de jura de cuentas previsto en el artículo 35 de la LEC.",
      "Un procedimiento monitorio o directamente un juicio verbal u ordinario en función de la cuantía.",
      "Cualquiera de los procedimientos reseñados en los apartados b) y c)."
    ],
    correct: 3
  },
  {
    section: "Deontología — Honorarios",
    text: "La minuta de honorarios de letrado incluida en la tasación de costas de un procedimiento ordinario es impugnada por excesiva. ¿Qué criterios se tendrán en cuenta en la emisión del dictamen colegial (ICAB)?",
    options: [
      "Los Criterios Orientadores en materia de honorarios profesionales a los exclusivos efectos de la tasación de costas y jura de cuentas, aprobados por la Junta de Gobierno en fecha 21 de diciembre de 2009.",
      "Se estará al pacto suscrito por la parte condenada en costas con su abogado.",
      "Las Pautas Básicas en materia de Criterios Orientadores de honorarios profesionales, aprobadas por Acuerdo de la Junta de Gobierno de 20 de marzo de 2018 (Derogadas).",
      "Ninguna de las respuestas anteriores es correcta."
    ],
    correct: 3
  },
  {
    section: "Deontología — Honorarios",
    text: "Juan, abogado de Laura, decide reclamar su minuta interponiendo un procedimiento monitorio sin haber suscrito hoja de encargo profesional. ¿Qué juzgado será competente territorialmente para tramitar la reclamación?",
    options: [
      "Siempre el del domicilio profesional del abogado.",
      "El Juzgado que tramitó el procedimiento ordinario de reclamación de cantidad.",
      "El Juzgado de Primera Instancia del domicilio o residencia del deudor, de acuerdo con el artículo 813 de la LEC.",
      "El del domicilio del abogado o el del domicilio del deudor, a elección del demandante."
    ],
    correct: 2
  },
  {
    section: "Deontología — Honorarios",
    text: "Si interponemos un procedimiento ordinario en reclamación de honorarios profesionales y la parte demandada contesta y se opone por considerar que los mismos son excesivos:",
    options: [
      "Siempre se dará traslado al Colegio de la Abogacía para que emita el informe previsto en el artículo 246.1 de la LEC.",
      "Se dará traslado al Colegio de la Abogacía para que emita el informe previsto en el artículo 246.1 de la LEC, si así lo solicitare alguna de las partes.",
      "Cualquiera de las partes puede solicitar un dictamen pericial judicial de acuerdo con los artículos 339 y siguientes de la LEC.",
      "No cabe la posibilidad de solicitar al Colegio de la Abogacía ni el informe previsto en el artículo 246.1 de LEC ni un dictamen pericial judicial."
    ],
    correct: 1
  },
  {
    section: "Deontología — Honorarios",
    text: "Si un cliente no nos paga y tenemos que reclamar judicialmente nuestros honorarios profesionales, ¿tenemos algún plazo para ejercitar la acción?",
    options: [
      "Un plazo de prescripción de diez años, de acuerdo con el artículo 121-20 del Código Civil de Cataluña.",
      "Un plazo de prescripción de tres años, de acuerdo con el artículo 121-21 b) del Código Civil de Cataluña.",
      "Un plazo de prescripción de un año, de acuerdo con el artículo 121-22 del Código Civil de Cataluña.",
      "No tenemos ningún plazo."
    ],
    correct: 1
  },
  {
    section: "Deontología — Cuota Litis",
    text: "Un cliente contacta con un abogado para proceder a la reclamación de la indemnización derivada de un accidente de tráfico, pactando unos honorarios del 10 por ciento de la indemnización. ¿Pueden abogado y cliente pactar unos honorarios que dependan exclusivamente de la indemnización efectivamente obtenida?",
    options: [
      "Sí, porque en la actualidad la cuota litis está permitida.",
      "No, porque ello puede generar un conflicto de intereses entre abogado y cliente.",
      "Sí, porque ningún tercero debe tener conocimiento de lo pactado con el cliente.",
      "No, porque se trata de un pacto de cuota litis prohibido por el artículo 16 del Código Deontológico."
    ],
    correct: 3
  },
  {
    section: "Deontología — Honorarios",
    text: "Ante el impago de sus honorarios pactados por escrito en hoja de encargo y derivados de un procedimiento civil de reclamación de cantidad, el abogado decide reclamar su minuta a través de un juicio ordinario. ¿Qué Juzgado es competente territorialmente para su tramitación?",
    options: [
      "Siempre el domicilio del demandado.",
      "El pactado entre las partes y a falta de este el domicilio del demandado.",
      "El Juzgado que tramitó la reclamación de cantidad.",
      "Siempre el domicilio del abogado."
    ],
    correct: 1
  },
  {
    section: "Deontología — Honorarios",
    text: "En la Tasación de Costas, si la impugnación de los honorarios por excesivos es total o parcialmente estimada, ¿hay imposición de costas?",
    options: [
      "Sí, se impondrán las costas al abogado cuyos honorarios se hubieran considerado excesivos.",
      "No, en los procesos de impugnación de honorarios no se imponen costas.",
      "Sólo se impondrán las costas si se aprecia temeridad o mala fe en el Letrado minutante.",
      "Sólo se impondrán las costas si el informe del Colegio de Abogados es favorable."
    ],
    correct: 0
  },
  {
    section: "Deontología — Honorarios",
    text: "¿Qué se puede hacer contra el Decreto dictado por el Letrado de la Administración de Justicia en relación a la impugnación por excesiva de la minuta del letrado dictado en un procedimiento de reclamación de honorarios del artículo 35 de la LEC?",
    options: [
      "Cabe interponer recurso de reposición y contra su resolución revisión.",
      "No cabe recurso alguno causando excepción de cosa juzgada.",
      "Cabe interponer recurso de revisión.",
      "No cabe recurso alguno, sin perjuicio de instar un procedimiento declarativo."
    ],
    correct: 3
  },
  // ───── BLANQUEO ─────
  {
    section: "Blanqueo de Capitales",
    text: "El Abogado Alberto asesora a Eduardo en la compraventa de un local en la Costa del Sol valorado en 360.000 €. Eduardo paga 30.000 € de señal y luego comunica que abonará el resto al contado sin financiación. ¿Tiene alguna obligación Alberto en relación con el blanqueo de capitales?",
    options: [
      "Sí, debe comunicar la operación al Servicio Ejecutivo de la Comisión de Prevención del Blanqueo de Capitales e Infracciones Monetarias.",
      "No, ya que los Abogados y Procuradores no son sujetos obligados en la Ley de prevención del blanqueo de capitales cuando actúan en representación de un cliente.",
      "No, ya que los Abogados son sujetos obligados, pero por excepción no están sujetos al deber de comunicación cuando asisten jurídicamente a su cliente, según la Ley de prevención del blanqueo de capitales y prevención del terrorismo.",
      "No, ya que actúa bajo secreto profesional, pues su intervención es en calidad de Abogado."
    ],
    correct: 0
  },
  {
    section: "Blanqueo de Capitales",
    text: "El SEPBLAC requiere al abogado José información sobre su cliente (Importaciones Kingston, SA). Una vez facilitada esa información al SEPBLAC, José se plantea avisar a su cliente de que está sometido a una inspección. Indique lo más correcto:",
    options: [
      "No debe informar a su cliente por estar prohibido por la Ley de prevención del blanqueo de capitales y de la financiación del terrorismo.",
      "Debe informar a su cliente, pues no puede ocultarle información sobre un expediente.",
      "Debe informar a su cliente, pero solo si todavía no se ha examinado por el SEPBLAC alguna operación.",
      "No debe informar a su cliente, por estar sujeto al secreto profesional."
    ],
    correct: 0
  },
  {
    section: "Blanqueo de Capitales",
    text: "Carmelo recibe a Juan Fernández Galindo, quien quiere adquirir un inmueble en la Costa del Sol. Juan reconoce que el comprador real es un ciudadano ruso llamado Anatoly, comprometiéndose a firmar los contratos y hacer los pagos, con la condición de que Anatoly no figure en ningún documento. ¿Puede seguir asesorando Carmelo a Juan?",
    options: [
      "No, ya que la legislación de blanqueo de capitales le obliga a recabar la información precisa a fin de determinar la exacta identidad de la persona por cuenta de la cual actúa Juan.",
      "Sí, ya que Juan está dispuesto a figurar como parte contratante, siendo Carmelo ajeno a las obligaciones que tenga suscritas con el tal Anatoly.",
      "No, ya que la legislación de blanqueo de capitales prohíbe a los abogados participar en el asesoramiento de operaciones por cuenta de clientes relativas a la compraventa de bienes inmuebles.",
      "Sí, porque los abogados no son sujetos obligados por la legislación de blanqueo, al estar amparados por el derecho al secreto profesional."
    ],
    correct: 0
  },
  {
    section: "Blanqueo de Capitales",
    text: "Nuria tiene una cuenta corriente en Berna (Suiza) y le pide a Antonio, abogado, que realice las gestiones necesarias para traer el dinero a España, manifestándole que el dinero proviene de la venta de una casa en dicha ciudad que acaba de heredar de una tía. ¿Qué debe hacer Antonio?",
    options: [
      "Abstenerse de llevar el asunto dado que los fondos se encuentran en un paraíso fiscal.",
      "Debe extremar la cautela y hacer un examen especial al tratarse de fondos provenientes de un paraíso fiscal.",
      "Debe informar al Servicio Ejecutivo de la Comisión de Prevención de Blanqueo de Capitales e Infracciones Monetarias por ser una operación sospechosa.",
      "Debe aceptar el encargo y confiar en lo manifestado por Nuria."
    ],
    correct: 3
  },
  {
    section: "Blanqueo de Capitales",
    text: "La letrada Estefanía está gestionando una operación de la cual se desprenden indicios de que se pueda estar utilizando para blanquear capitales. ¿Debe la Letrada realizar algún tipo de comunicación?",
    options: [
      "Sí, debe informar al Colegio de Abogados al que pertenece.",
      "Sí, debe informar al Servicio Ejecutivo de la Comisión de Prevención de Blanqueo de Capitales e Infracciones Monetarias.",
      "No, basta con la realización de un informe que señale el riesgo y se adjunte al expediente junto con la identificación formal del cliente.",
      "Sí, debe informar a la Cámara de Prevención de Blanqueo de Capitales Española."
    ],
    correct: 1
  },
  {
    section: "Blanqueo de Capitales (2021)",
    text: "La obligación de los Abogados de conservar la documentación en que se formalice el cumplimiento de las obligaciones que se les imponen como sujetos obligados en virtud de la Ley 10/2010 se extiende durante:",
    options: ["Diez años.", "Veinte años.", "Cuatro años.", "Quince años."],
    correct: 0
  },
  {
    section: "Blanqueo de Capitales (2022)",
    text: "El Abogado Víctor recibe un requerimiento de información por parte del SEPBLAC para que aporte determinada documentación facilitada por su cliente para su defensa en un procedimiento judicial sobre la propiedad de unas participaciones societarias. ¿Cuál de las siguientes conductas es correcta?",
    options: [
      "El Abogado deberá facilitar toda la información requerida salvo que el cliente lo haya excluido expresamente en la hoja de encargo.",
      "El Abogado deberá facilitar toda la información requerida en todo caso.",
      "El Abogado deberá facilitar toda la información requerida, una vez lo haya comunicado fehacientemente a su cliente.",
      "El Abogado no estará sometido a la obligación de facilitar la documentación requerida con respecto a la información que reciba en la defensa de procesos judiciales."
    ],
    correct: 3
  },
  {
    section: "Blanqueo de Capitales (2022)",
    text: "Estefanía defiende los intereses de un empresario ruso sobre operaciones relativas a la compraventa de inmuebles. Este empresario se sirve de un intermediario para hacerle los encargos a Estefanía y pagarle los servicios prestados. ¿Puede Estefanía gestionar estos servicios del empresario ruso teniendo en cuenta lo previsto en la Ley 10/2010?",
    options: [
      "No puede gestionar los servicios en ningún caso, pues no le conoce personalmente.",
      "No puede gestionar los servicios en ningún caso, puesto que no tiene certeza de que le pague el propio empresario ruso.",
      "Sí puede gestionar los servicios en todo caso, sin tener que realizar ninguna actuación al respecto.",
      "Sí puede gestionar los servicios siempre que el intermediario le facilite los datos del titular real de la operación y le exhiba los documentos oportunos."
    ],
    correct: 3
  }
];

// ── State ──
let currentIdx = 0;
let answers = new Array(QUESTIONS.length).fill(null); // null | 'correct' | 'incorrect'
let scoreOk = 0, scoreBad = 0;
const TOTAL = QUESTIONS.length;
const LETTERS = ['A', 'B', 'C', 'D'];

function startQuiz() {
  document.getElementById('splash').style.display = 'none';
  document.getElementById('app').style.display = 'flex';
  renderQ(0, null);
  updateHeader();
}

function renderQ(idx, direction) {
  const q = QUESTIONS[idx];
  const slide = document.getElementById('current-slide');
  const isAnswered = answers[idx] !== null;

  // Build HTML
  const opts = q.options.map((opt, j) => {
    let cls = '';
    if (isAnswered) {
      if (j === q.correct) cls = 'correct';
      else if (answers[idx] === j) cls = 'incorrect';
    }
    return `<button class="opt-btn ${cls}" onclick="answer(${idx},${j})" ${isAnswered ? 'disabled' : ''}>
      <span class="opt-circle">${LETTERS[j]}</span>
      <span>${opt}</span>
    </button>`;
  }).join('');

  let fbHtml = '';
  if (isAnswered) {
    if (answers[idx] === q.correct) {
      fbHtml = `<div class="feedback-banner ok show"><span class="fb-icon">✅</span> ¡Correcto!</div>`;
    } else {
      fbHtml = `<div class="feedback-banner fail show"><span class="fb-icon">❌</span> Incorrecto — la respuesta correcta está en verde.</div>`;
    }
  }

  slide.innerHTML = `
    <div class="q-num">Pregunta ${idx + 1} de ${TOTAL}</div>
    <div class="q-text">${q.text}</div>
    <div class="options">${opts}</div>
    ${fbHtml}
  `;

  // Animate
  if (direction === 'next') {
    slide.classList.remove('active', 'enter-right', 'enter-left', 'exit-left', 'exit-right');
    slide.classList.add('enter-right');
    requestAnimationFrame(() => {
      requestAnimationFrame(() => {
        slide.classList.remove('enter-right');
        slide.classList.add('active');
      });
    });
  } else if (direction === 'prev') {
    slide.classList.remove('active', 'enter-right', 'enter-left', 'exit-left', 'exit-right');
    slide.classList.add('enter-left');
    requestAnimationFrame(() => {
      requestAnimationFrame(() => {
        slide.classList.remove('enter-left');
        slide.classList.add('active');
      });
    });
  } else {
    slide.classList.add('active');
  }

  updateHeader();
  updateNav();
}

function answer(idx, optIdx) {
  if (answers[idx] !== null) return;
  const q = QUESTIONS[idx];
  answers[idx] = optIdx;
  if (optIdx === q.correct) scoreOk++; else scoreBad++;
  updateHeader();
  renderQ(idx, null);
  // Auto-enable next btn
  document.getElementById('btn-next').disabled = false;
}

function handleNext() {
  if (currentIdx < TOTAL - 1) {
    const prev = currentIdx;
    currentIdx++;
    renderQ(currentIdx, 'next');
  } else {
    showResults();
  }
}

function goTo(idx) {
  if (idx < 0 || idx >= TOTAL) return;
  const dir = idx > currentIdx ? 'next' : 'prev';
  currentIdx = idx;
  renderQ(currentIdx, dir);
}

function updateHeader() {
  document.getElementById('sc-ok').textContent = scoreOk;
  document.getElementById('sc-bad').textContent = scoreBad;
  const answered = answers.filter(a => a !== null).length;
  const pct = (answered / TOTAL) * 100;
  document.getElementById('prog-fill').style.width = pct + '%';
  document.getElementById('prog-label').textContent = `${answered} / ${TOTAL}`;
  document.getElementById('section-badge').textContent = QUESTIONS[currentIdx].section;
}

function updateNav() {
  const btnPrev = document.getElementById('btn-prev');
  const btnNext = document.getElementById('btn-next');
  btnPrev.disabled = currentIdx === 0;
  const isAnswered = answers[currentIdx] !== null;
  if (currentIdx === TOTAL - 1) {
    btnNext.textContent = isAnswered ? 'Ver resultados →' : 'Siguiente →';
    btnNext.disabled = !isAnswered;
  } else {
    btnNext.textContent = 'Siguiente →';
    btnNext.disabled = !isAnswered;
  }
}

function showResults() {
  const pct = Math.round((scoreOk / TOTAL) * 100);
  let emoji = pct >= 75 ? '🎉' : pct >= 50 ? '💪' : '📚';
  let msg = pct >= 75 ? '¡Excelente resultado!' : pct >= 50 ? '¡Buen intento!' : 'Sigue practicando';

  // Conic gradient for ring
  const deg = Math.round((scoreOk / TOTAL) * 360);

  const rs = document.getElementById('results-screen');
  rs.style.cssText = 'display:flex;flex-direction:column;align-items:center;justify-content:center;position:fixed;inset:0;background:var(--bg);padding:32px 24px;text-align:center;z-index:200;overflow-y:auto;';
  rs.innerHTML = `
    <div style="font-size:3rem;margin-bottom:16px">${emoji}</div>
    <div class="result-title">${msg}</div>
    <div class="result-detail">${pct}% de aciertos<br>${scoreOk} correctas · ${scoreBad} incorrectas · ${TOTAL} preguntas</div>
    <div class="stat-row" style="display:flex;gap:14px;margin-bottom:32px;width:100%;max-width:360px">
      <div class="stat-box ok" style="flex:1;padding:16px;border-radius:14px;border:1.5px solid var(--green-border);background:var(--green-bg);text-align:center">
        <div class="stat-num" style="font-size:2rem;font-weight:800;color:var(--green-text);line-height:1;margin-bottom:4px">${scoreOk}</div>
        <div class="stat-lbl" style="font-size:0.7rem;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:var(--muted)">Correctas</div>
      </div>
      <div class="stat-box bad" style="flex:1;padding:16px;border-radius:14px;border:1.5px solid var(--red-border);background:var(--red-bg);text-align:center">
        <div class="stat-num" style="font-size:2rem;font-weight:800;color:var(--red-text);line-height:1;margin-bottom:4px">${scoreBad}</div>
        <div class="stat-lbl" style="font-size:0.7rem;font-weight:700;letter-spacing:0.08em;text-transform:uppercase;color:var(--muted)">Incorrectas</div>
      </div>
    </div>
    <button class="restart-btn" onclick="restart()" style="width:100%;max-width:360px;padding:16px;border-radius:14px;border:none;background:linear-gradient(135deg,var(--purple),var(--gold));color:#fff;font-family:Sora,sans-serif;font-size:1rem;font-weight:700;cursor:pointer">🔄 Repetir test</button>
  `;
}

function restart() {
  answers = new Array(TOTAL).fill(null);
  scoreOk = 0; scoreBad = 0;
  currentIdx = 0;
  document.getElementById('results-screen').style.display = 'none';
  renderQ(0, null);
  updateHeader();
  updateNav();
}
</script>
</body>
</html>

