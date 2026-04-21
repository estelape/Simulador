<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Simulador de Entrevista Clínica</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #F5F2ED;
    --surface: #FFFFFF;
    --surface2: #EEE9E2;
    --border: #D8D0C5;
    --text: #1C1712;
    --text2: #6B6258;
    --text3: #9C9188;
    --accent: #3D5A4C;
    --accent2: #6B9E84;
    --accent-light: #E8F0EB;
    --patient-bg: #F0EDE8;
    --student-bg: #3D5A4C;
    --student-text: #FFFFFF;
    --warn: #8B4513;
    --warn-bg: #FDF4EE;
    --tag-bg: #E8F0EB;
    --tag-text: #2D4A3C;
    --radius: 12px;
    --radius-sm: 8px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem 1rem;
  }

  .header {
    text-align: center;
    margin-bottom: 2rem;
    max-width: 680px;
    width: 100%;
  }

  .header-tag {
    display: inline-block;
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--accent);
    background: var(--accent-light);
    padding: 4px 12px;
    border-radius: 20px;
    margin-bottom: 0.75rem;
  }

  .header h1 {
    font-family: 'DM Serif Display', serif;
    font-size: 2rem;
    color: var(--text);
    line-height: 1.2;
    margin-bottom: 0.4rem;
  }

  .header p {
    font-size: 14px;
    color: var(--text2);
    font-weight: 300;
  }

  .main-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 20px;
    padding: 1.75rem;
    max-width: 680px;
    width: 100%;
    box-shadow: 0 2px 20px rgba(0,0,0,0.06);
  }

  /* Student ID */
  .student-section {
    display: flex;
    gap: 10px;
    align-items: flex-end;
    margin-bottom: 1.5rem;
    padding-bottom: 1.5rem;
    border-bottom: 1px solid var(--border);
  }

  .field-group { flex: 1; }
  .field-label {
    font-size: 11px;
    font-weight: 500;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    color: var(--text3);
    margin-bottom: 6px;
    display: block;
  }

  input[type="text"], select {
    width: 100%;
    padding: 9px 12px;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    background: var(--bg);
    color: var(--text);
    outline: none;
    transition: border-color 0.2s;
  }

  input[type="text"]:focus, select:focus { border-color: var(--accent2); }

  /* Profile bar */
  .profile-bar {
    display: flex;
    align-items: center;
    gap: 14px;
    background: var(--patient-bg);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1rem 1.25rem;
    margin-bottom: 1.25rem;
  }

  .avatar {
    width: 46px; height: 46px;
    border-radius: 50%;
    background: var(--accent);
    color: #fff;
    display: flex; align-items: center; justify-content: center;
    font-weight: 500; font-size: 15px;
    flex-shrink: 0;
  }

  .profile-info { flex: 1; }
  .profile-name { font-size: 15px; font-weight: 500; margin-bottom: 2px; }
  .profile-meta { font-size: 12px; color: var(--text2); line-height: 1.4; }

  .badge {
    font-size: 11px; font-weight: 500;
    padding: 4px 10px;
    border-radius: 20px;
    background: var(--tag-bg);
    color: var(--tag-text);
    white-space: nowrap;
  }

  /* Chat */
  .chat-area {
    border: 1px solid var(--border);
    border-radius: var(--radius);
    background: var(--bg);
    min-height: 280px;
    max-height: 360px;
    overflow-y: auto;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin-bottom: 1rem;
  }

  .msg {
    max-width: 80%;
    padding: 10px 14px;
    font-size: 14px;
    line-height: 1.6;
    border-radius: 14px;
  }

  .msg.patient {
    align-self: flex-start;
    background: var(--surface);
    color: var(--text);
    border: 1px solid var(--border);
    border-bottom-left-radius: 4px;
  }

  .msg.student {
    align-self: flex-end;
    background: var(--student-bg);
    color: var(--student-text);
    border-bottom-right-radius: 4px;
  }

  .msg.system {
    align-self: center;
    font-size: 11px;
    color: var(--text3);
    background: none;
    text-align: center;
    padding: 2px 0;
    letter-spacing: 0.03em;
  }

  .msg.loading {
    align-self: flex-start;
    background: var(--surface);
    border: 1px solid var(--border);
    border-bottom-left-radius: 4px;
    color: var(--text3);
    font-style: italic;
    font-size: 13px;
  }

  .typing-dots { display: inline-flex; gap: 4px; align-items: center; }
  .typing-dots span {
    width: 5px; height: 5px; border-radius: 50%;
    background: var(--text3);
    animation: dot 1.2s infinite;
  }
  .typing-dots span:nth-child(2) { animation-delay: 0.2s; }
  .typing-dots span:nth-child(3) { animation-delay: 0.4s; }
  @keyframes dot { 0%,80%,100%{opacity:0.3;transform:scale(0.8)} 40%{opacity:1;transform:scale(1)} }

  /* Input */
  .input-row { display: flex; gap: 8px; margin-bottom: 1rem; }

  textarea {
    flex: 1; resize: none;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 10px 12px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    background: var(--bg);
    color: var(--text);
    line-height: 1.5;
    outline: none;
    transition: border-color 0.2s;
  }
  textarea:focus { border-color: var(--accent2); }

  .btn-send {
    padding: 10px 20px;
    background: var(--accent);
    color: #fff;
    border: none;
    border-radius: var(--radius-sm);
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    transition: background 0.2s, transform 0.1s;
    align-self: flex-end;
  }
  .btn-send:hover { background: #2D4A3C; }
  .btn-send:active { transform: scale(0.97); }
  .btn-send:disabled { opacity: 0.45; cursor: not-allowed; }

  /* Stats */
  .stats-row {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 8px;
    margin-bottom: 1rem;
  }

  .stat-card {
    background: var(--surface2);
    border-radius: var(--radius-sm);
    padding: 10px 8px;
    text-align: center;
  }

  .stat-num { font-size: 22px; font-weight: 500; color: var(--text); }
  .stat-label { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.06em; margin-top: 2px; }

  /* Divider */
  .divider {
    border: none;
    border-top: 1px solid var(--border);
    margin: 1.25rem 0;
  }

  /* Autoevaluation */
  .autoeval-section { display: none; }
  .autoeval-section.visible { display: block; }

  .autoeval-title {
    font-family: 'DM Serif Display', serif;
    font-size: 1.1rem;
    margin-bottom: 0.25rem;
  }

  .autoeval-sub { font-size: 13px; color: var(--text2); margin-bottom: 1.25rem; }

  .question-item { margin-bottom: 1.1rem; }
  .question-text { font-size: 13px; font-weight: 500; margin-bottom: 8px; color: var(--text); }

  .scale-row {
    display: flex;
    gap: 6px;
    align-items: center;
  }

  .scale-label { font-size: 11px; color: var(--text3); min-width: 70px; }
  .scale-label.right { text-align: right; }

  .scale-btns { display: flex; gap: 4px; flex: 1; justify-content: center; }

  .scale-btn {
    width: 34px; height: 34px;
    border-radius: 50%;
    border: 1px solid var(--border);
    background: var(--bg);
    color: var(--text2);
    font-size: 12px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.15s;
    font-family: 'DM Sans', sans-serif;
  }
  .scale-btn:hover { border-color: var(--accent2); color: var(--accent); }
  .scale-btn.selected { background: var(--accent); border-color: var(--accent); color: #fff; }

  /* Buttons bottom */
  .actions-row { display: flex; gap: 8px; flex-wrap: wrap; }

  .btn-outline {
    flex: 1;
    padding: 10px 14px;
    border: 1px solid var(--border);
    background: var(--surface);
    border-radius: var(--radius-sm);
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    color: var(--text2);
    cursor: pointer;
    transition: all 0.15s;
    text-align: center;
  }
  .btn-outline:hover { border-color: var(--accent2); color: var(--accent); background: var(--accent-light); }

  .btn-primary-outline {
    flex: 1;
    padding: 10px 14px;
    border: 1px solid var(--accent);
    background: var(--accent-light);
    border-radius: var(--radius-sm);
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    font-weight: 500;
    color: var(--accent);
    cursor: pointer;
    transition: all 0.15s;
    text-align: center;
  }
  .btn-primary-outline:hover { background: var(--accent); color: #fff; }

  .copy-toast {
    position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%);
    background: var(--accent); color: #fff;
    padding: 10px 20px; border-radius: 20px;
    font-size: 13px; font-weight: 500;
    opacity: 0; transition: opacity 0.3s;
    pointer-events: none;
  }
  .copy-toast.show { opacity: 1; }

  /* New session link */
  .new-session {
    text-align: center;
    margin-top: 1rem;
    font-size: 13px;
    color: var(--text3);
  }
  .new-session a { color: var(--accent2); cursor: pointer; text-decoration: none; }
  .new-session a:hover { text-decoration: underline; }

  @media(max-width:480px){
    .stats-row { grid-template-columns: repeat(2,1fr); }
    .header h1 { font-size: 1.5rem; }
    body { padding: 1rem 0.75rem; }
  }
</style>
</head>
<body>

<div class="header">
  <span class="header-tag">Práctica de habilidades clínicas</span>
  <h1>Simulador de Entrevista</h1>
  <p>Entrevista a un paciente virtual y recibe retroalimentación al finalizar</p>
</div>

<div class="main-card">

  <!-- Student ID -->
  <div class="student-section">
    <div class="field-group">
      <label class="field-label">Nombre del alumno</label>
      <input type="text" id="studentName" placeholder="Tu nombre completo">
    </div>
    <div class="field-group" style="max-width:220px;">
      <label class="field-label">Caso clínico</label>
      <select id="caseSelect" onchange="loadCase()">
        <option value="ansiedad">Ansiedad Generalizada</option>
        <option value="depresion">Episodio Depresivo Mayor</option>
        <option value="duelo">Duelo Complicado</option>
        <option value="fobia">Fobia Específica</option>
      </select>
    </div>
  </div>

  <!-- Patient profile -->
  <div class="profile-bar">
    <div class="avatar" id="avatarEl">MG</div>
    <div class="profile-info">
      <p class="profile-name" id="patientName">María García, 34 años</p>
      <p class="profile-meta" id="patientMeta">Motivo de consulta: preocupación excesiva y tensión constante desde hace 2 años</p>
    </div>
    <span class="badge" id="caseBadge">TAG</span>
  </div>

  <!-- Chat -->
  <div class="chat-area" id="chatArea">
    <div class="msg system">— Sesión iniciada · Saluda al paciente para comenzar —</div>
  </div>

  <!-- Input -->
  <div class="input-row">
    <textarea id="userInput" rows="2" placeholder="Escribe tu pregunta o intervención… (Enter para enviar)" onkeydown="handleKey(event)"></textarea>
    <button class="btn-send" id="sendBtn" onclick="sendMessage()">Enviar</button>
  </div>

  <!-- Stats -->
  <div class="stats-row">
    <div class="stat-card"><div class="stat-num" id="statQ">0</div><div class="stat-label">Preguntas</div></div>
    <div class="stat-card"><div class="stat-num" id="statOpen">0</div><div class="stat-label">Abiertas</div></div>
    <div class="stat-card"><div class="stat-num" id="statClosed">0</div><div class="stat-label">Cerradas</div></div>
    <div class="stat-card"><div class="stat-num" id="statTurns">0</div><div class="stat-label">Turnos</div></div>
  </div>

  <hr class="divider">

  <!-- Autoevaluation -->
  <div class="autoeval-section" id="autoevalSection">
    <p class="autoeval-title">Autoevaluación</p>
    <p class="autoeval-sub">Antes de ver la retroalimentación de la IA, evalúa tu propia entrevista (1 = muy poco, 5 = muy bien)</p>

    <div id="autoevalQuestions"></div>
    <hr class="divider">
  </div>

  <!-- Actions -->
  <div class="actions-row">
    <button class="btn-outline" onclick="toggleAutoevaluation()">📋 Autoevaluarme primero</button>
    <button class="btn-outline" onclick="copyTranscript()">📄 Copiar transcripción</button>
    <button class="btn-primary-outline" onclick="requestFeedback()">Solicitar retroalimentación ↗</button>
  </div>

  <div class="new-session">
    <a onclick="resetChat()">+ Iniciar nueva sesión</a>
  </div>

</div>

<div class="copy-toast" id="copyToast">¡Transcripción copiada!</div>

<script>
const AUTOEVAL_QUESTIONS = [
  { id: 'q1', text: '¿Qué tan bien logré establecer rapport y crear un ambiente de confianza?' },
  { id: 'q2', text: '¿Utilicé preguntas abiertas para explorar la experiencia del paciente?' },
  { id: 'q3', text: '¿Exploré adecuadamente los síntomas, su duración e impacto en la vida diaria?' },
  { id: 'q4', text: '¿Practiqué escucha activa y respondí con empatía?' },
  { id: 'q5', text: '¿Manejé bien los silencios y los momentos de incomodidad del paciente?' }
];

const cases = {
  ansiedad: {
    name: "María García, 34 años", initials: "MG", badge: "TAG",
    meta: "Motivo de consulta: preocupación excesiva y tensión constante desde hace 2 años",
    system: `Eres María García, una mujer de 34 años que acude a consulta psicológica por primera vez. Tienes Trastorno de Ansiedad Generalizada (TAG) no diagnosticado. Tus síntomas incluyen: preocupación excesiva e incontrolable por múltiples áreas (trabajo, familia, salud), tensión muscular, dificultad para concentrarte, irritabilidad, problemas para dormir (te cuesta conciliar el sueño y te despiertas en la noche). Llevas así aproximadamente 2 años. Eres contadora, casada, con una hija de 7 años. Eres reservada al principio pero te abres con el tiempo. Respondes de forma natural y humana, no das toda la información de golpe. Si el entrevistador usa preguntas cerradas, respondes brevemente (sí/no/poco). Si usa preguntas abiertas, te explayas más. No menciones diagnósticos ni términos clínicos. Habla en español mexicano informal. Máximo 3-4 oraciones por respuesta.`
  },
  depresion: {
    name: "Carlos Mendoza, 41 años", initials: "CM", badge: "EDM",
    meta: "Motivo de consulta: tristeza profunda y pérdida de interés en actividades",
    system: `Eres Carlos Mendoza, un hombre de 41 años que asiste a consulta por insistencia de su esposa. Tienes un Episodio Depresivo Mayor moderado. Tus síntomas: tristeza persistente casi todo el día, anhedonia (ya no disfrutas el futbol ni salir con amigos), hipersomnia (duermes mucho pero no descansas), fatiga intensa, sentimientos de inutilidad, dificultad para concentrarte en el trabajo. Llevas aproximadamente 5 meses así. Trabajas como ingeniero civil. Eres escéptico respecto a la terapia. Hablas poco, respuestas cortas a menos que te den confianza. Habla en español mexicano, tono neutro-resignado. Máximo 3 oraciones.`
  },
  duelo: {
    name: "Sofía Ramírez, 55 años", initials: "SR", badge: "Duelo",
    meta: "Motivo de consulta: dificultad para aceptar la pérdida de su esposo (10 meses)",
    system: `Eres Sofía Ramírez, 55 años, viuda hace 10 meses. Tu esposo murió de infarto fulminante. Presentas duelo complicado: sigues hablando de él en presente, conservas todo igual en casa, evitas salir, lloras frecuentemente, sientes que "no tiene caso" seguir. Tienes dos hijos adultos que viven fuera. Fuiste ama de casa toda tu vida. Eres cálida pero frágil emocionalmente. Te puede costar hablar del tema pero cuando lo haces te emocionas. Habla en español mexicano, cálido y nostálgico. Máximo 3-4 oraciones.`
  },
  fobia: {
    name: "Diego Torres, 22 años", initials: "DT", badge: "Fobia",
    meta: "Motivo de consulta: miedo intenso a conducir tras un accidente de tráfico",
    system: `Eres Diego Torres, 22 años, estudiante universitario. Desarrollaste fobia a conducir tras un accidente de tráfico hace 8 meses (sin heridos graves pero muy impactante). Síntomas: ansiedad intensa al ver autos, evitas subirte a vehículos, taquicardia y sudoración solo de pensarlo, pesadillas ocasionales. Esto afecta tu vida: ya no vas a la uni en carro, dependes de tus papás. Te da vergüenza hablar de esto. Eres joven, informal. Al principio minimizas. Habla en español mexicano juvenil. Máximo 3 oraciones.`
  }
};

let history = [];
let currentCase = 'ansiedad';
let questionCount = 0, openCount = 0, closedCount = 0, turnCount = 0;
let autoevalVisible = false;
const autoevalAnswers = {};

function loadCase() {
  currentCase = document.getElementById('caseSelect').value;
  resetChat();
}

function resetChat() {
  currentCase = document.getElementById('caseSelect').value;
  const c = cases[currentCase];
  history = [];
  questionCount = 0; openCount = 0; closedCount = 0; turnCount = 0;
  updateStats();
  document.getElementById('patientName').textContent = c.name;
  document.getElementById('patientMeta').textContent = c.meta;
  document.getElementById('caseBadge').textContent = c.badge;
  document.getElementById('avatarEl').textContent = c.initials;
  document.getElementById('chatArea').innerHTML = '<div class="msg system">— Sesión iniciada · Saluda al paciente para comenzar —</div>';
  document.getElementById('autoevalSection').classList.remove('visible');
  autoevalVisible = false;
}

function handleKey(e) {
  if (e.key === 'Enter' && !e.shiftKey) { e.preventDefault(); sendMessage(); }
}

function updateStats() {
  document.getElementById('statQ').textContent = questionCount;
  document.getElementById('statOpen').textContent = openCount;
  document.getElementById('statClosed').textContent = closedCount;
  document.getElementById('statTurns').textContent = turnCount;
}

function addMsg(text, role) {
  const chat = document.getElementById('chatArea');
  const div = document.createElement('div');
  div.className = 'msg ' + role;
  if (role === 'loading') {
    div.innerHTML = '<span class="typing-dots"><span></span><span></span><span></span></span>';
  } else {
    div.textContent = text;
  }
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
  return div;
}

function classifyQuestion(text) {
  const t = text.trim();
  const isQ = t.includes('?') || /^(qué|cómo|cuándo|cuánto|dónde|quién|por qué|cuál|platíca|cuéntame|explica|describe|háblame|dígame|dime)/i.test(t);
  if (!isQ) return null;
  const closedPat = /^(¿?(tiene|has|hay|es|son|fue|puedes|puede|sientes?|tienes?|vives?|duermes?|comes?|trabajas?|está|están|has tenido|ha tenido|se siente|se ha)\b)/i;
  return closedPat.test(t) ? 'closed' : 'open';
}

async function sendMessage() {
  const inp = document.getElementById('userInput');
  const text = inp.value.trim();
  if (!text) return;
  inp.value = '';
  document.getElementById('sendBtn').disabled = true;

  addMsg(text, 'student');
  history.push({ role: 'user', content: text });

  const qType = classifyQuestion(text);
  if (qType) {
    questionCount++;
    if (qType === 'open') openCount++; else closedCount++;
  }
  turnCount++;
  updateStats();

  const loadDiv = addMsg('', 'loading');
  const c = cases[currentCase];

  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        system: c.system,
        messages: history
      })
    });
    const data = await res.json();
    const reply = data.content?.map(b => b.text || '').join('') || 'Lo siento, no pude responder.';
    loadDiv.remove();
    addMsg(reply, 'patient');
    history.push({ role: 'assistant', content: reply });
  } catch(e) {
    loadDiv.remove();
    addMsg('Error de conexión. Verifica tu internet e intenta de nuevo.', 'system');
  }
  document.getElementById('sendBtn').disabled = false;
}

function toggleAutoevaluation() {
  const section = document.getElementById('autoevalSection');
  const qContainer = document.getElementById('autoevalQuestions');
  if (!autoevalVisible) {
    qContainer.innerHTML = '';
    AUTOEVAL_QUESTIONS.forEach(q => {
      const div = document.createElement('div');
      div.className = 'question-item';
      div.innerHTML = `
        <p class="question-text">${q.text}</p>
        <div class="scale-row">
          <span class="scale-label">Poco</span>
          <div class="scale-btns" id="scale_${q.id}">
            ${[1,2,3,4,5].map(n => `<button class="scale-btn" onclick="selectScale('${q.id}', ${n}, this)">${n}</button>`).join('')}
          </div>
          <span class="scale-label right">Muy bien</span>
        </div>
      `;
      qContainer.appendChild(div);
    });
    section.classList.add('visible');
    autoevalVisible = true;
  } else {
    section.classList.remove('visible');
    autoevalVisible = false;
  }
}

function selectScale(qId, value, btn) {
  autoevalAnswers[qId] = value;
  const btns = document.querySelectorAll(`#scale_${qId} .scale-btn`);
  btns.forEach(b => b.classList.remove('selected'));
  btn.classList.add('selected');
}

function copyTranscript() {
  const studentName = document.getElementById('studentName').value || 'Alumno';
  const c = cases[currentCase];
  let text = `TRANSCRIPCIÓN DE ENTREVISTA CLÍNICA\n`;
  text += `Alumno: ${studentName}\n`;
  text += `Caso: ${c.name} — ${c.badge}\n`;
  text += `Preguntas: ${questionCount} (${openCount} abiertas, ${closedCount} cerradas) | Turnos: ${turnCount}\n`;
  text += `${'─'.repeat(50)}\n\n`;
  history.forEach(m => {
    text += (m.role === 'user' ? 'Entrevistador: ' : 'Paciente: ') + m.content + '\n\n';
  });
  if (Object.keys(autoevalAnswers).length > 0) {
    text += `${'─'.repeat(50)}\nAUTOEVALUACIÓN\n`;
    AUTOEVAL_QUESTIONS.forEach(q => {
      text += `${q.text}\nRespuesta: ${autoevalAnswers[q.id] || 'Sin responder'}/5\n\n`;
    });
  }
  navigator.clipboard.writeText(text).then(() => {
    const toast = document.getElementById('copyToast');
    toast.classList.add('show');
    setTimeout(() => toast.classList.remove('show'), 2500);
  });
}

function requestFeedback() {
  const studentName = document.getElementById('studentName').value || 'un alumno';
  const c = cases[currentCase];
  let autoevalText = '';
  if (Object.keys(autoevalAnswers).length > 0) {
    autoevalText = '\n\nAUTOEVALUACIÓN DEL ALUMNO (escala 1-5):\n';
    AUTOEVAL_QUESTIONS.forEach(q => {
      autoevalText += `- ${q.text}: ${autoevalAnswers[q.id] || 'Sin responder'}/5\n`;
    });
  }
  const prompt = `Soy ${studentName}, estudiante de psicología clínica. Acabo de realizar una entrevista simulada con el siguiente caso: ${c.name} (${c.badge}).

Estadísticas de mi entrevista:
- Total de preguntas: ${questionCount}
- Preguntas abiertas: ${openCount}
- Preguntas cerradas: ${closedCount}
- Turnos totales: ${turnCount}
${autoevalText}

TRANSCRIPCIÓN COMPLETA:
${history.map(m => (m.role === 'user' ? 'Entrevistador: ' : 'Paciente: ') + m.content).join('\n\n')}

Por favor, dame retroalimentación estructurada sobre:
1. Uso de preguntas abiertas vs cerradas y su impacto en la entrevista
2. Habilidades de rapport y empatía demostradas
3. Exploración de síntomas (profundidad, áreas cubiertas, áreas faltantes)
4. Manejo del espacio conversacional (silencios, seguimiento de respuestas)
5. Tres fortalezas concretas y tres áreas de mejora específicas
6. Una recomendación de práctica para mi próxima sesión`;

  if (typeof sendPrompt === 'function') {
    sendPrompt(prompt);
  } else {
    copyTranscript();
    alert('Copia la transcripción y pégala en tu plataforma de IA para recibir retroalimentación.');
  }
}
</script>
</body>
</html>
