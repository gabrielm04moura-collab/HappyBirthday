/* =========================================================
   JOVENSFLIX — Lógica de Aniversários
   ---------------------------------------------------------
   COMO EDITAR:
   - Para adicionar uma pessoa: copie um objeto do array
     abaixo e altere os campos.
   - "data"      → formato "DD/MM" (dia e mês).
   - "foto_url"  → link público da imagem. Pode ser:
                     • URL do GitHub (raw.githubusercontent.com/...)
                     • URL do Google Drive no formato:
                       https://drive.google.com/uc?id=ID_DO_ARQUIVO
                     • Ou deixe vazio "" para usar um avatar
                       gerado automaticamente com a inicial.
   - "legenda"   → frase curta que aparece embaixo do nome.
                   Deixe "" se não quiser mostrar nada.
   ========================================================= */

const JOVENS = [
  { nome: "Ana Luyza",      data: "30/10", foto_url: "", legenda: "" },
  { nome: "Davi",           data: "16/08", foto_url: "", legenda: "" },
  { nome: "Gabriel",        data: "24/06", foto_url: "", legenda: "" },
  { nome: "Maria Gabriele", data: "27/08", foto_url: "", legenda: "" },
  { nome: "Matheus",        data: "13/01", foto_url: "", legenda: "" },
  { nome: "Ramon",          data: "19/11", foto_url: "", legenda: "" },
  { nome: "Raphaella",      data: "16/01", foto_url: "", legenda: "" },
];

/* ---------- Paletas para placeholders ---------- */
const GRADIENTS = [
  "linear-gradient(135deg, #6a0510 0%, #E50914 100%)",
  "linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #E50914 100%)",
  "linear-gradient(135deg, #2c0708 0%, #831010 55%, #E50914 100%)",
  "linear-gradient(135deg, #0d0d0d 0%, #3d0a0a 50%, #E50914 100%)",
  "linear-gradient(135deg, #4a0e0e 0%, #B0060F 100%)",
  "linear-gradient(135deg, #1a1a1a 0%, #5c0e14 100%)",
  "linear-gradient(135deg, #2a0508 0%, #E50914 70%, #ff3742 100%)",
  "linear-gradient(135deg, #200000 0%, #6a0510 60%, #E50914 100%)",
];

function pickGradient(nome) {
  let h = 0;
  for (let i = 0; i < nome.length; i++) h = (h * 31 + nome.charCodeAt(i)) >>> 0;
  return GRADIENTS[h % GRADIENTS.length];
}

/* ---------- Datas e contagem ---------- */
function parseData(str) {
  const [d, m] = str.split("/").map(Number);
  return { dia: d, mes: m };
}

function isHoje(dia, mes) {
  const now = new Date();
  return now.getDate() === dia && (now.getMonth() + 1) === mes;
}

function proximoAniversario(dia, mes) {
  const now = new Date();

  // Se é o aniversário hoje, alvo = fim do dia (23:59:59)
  if (isHoje(dia, mes)) {
    return new Date(now.getFullYear(), mes - 1, dia, 23, 59, 59);
  }

  let target = new Date(now.getFullYear(), mes - 1, dia, 0, 0, 0);
  if (target <= now) {
    target = new Date(now.getFullYear() + 1, mes - 1, dia, 0, 0, 0);
  }
  return target;
}

function calcularContagem(target) {
  const now = new Date();
  if (target <= now) return { meses: 0, dias: 0, horas: 0, minutos: 0, total: 0 };

  // Conta meses inteiros somando 1 mês até passar do alvo
  let meses = 0;
  let cursor = new Date(now);
  while (true) {
    const next = new Date(cursor);
    next.setMonth(next.getMonth() + 1);
    if (next <= target) { meses++; cursor = next; } else break;
  }

  let ms = target - cursor;
  const dias    = Math.floor(ms / 86400000); ms -= dias    * 86400000;
  const horas   = Math.floor(ms / 3600000);  ms -= horas   * 3600000;
  const minutos = Math.floor(ms / 60000);

  return { meses, dias, horas, minutos, total: target - now };
}

const MESES_PT = ["jan","fev","mar","abr","mai","jun","jul","ago","set","out","nov","dez"];

function formatarData(dia, mes) {
  return `${String(dia).padStart(2, "0")} ${MESES_PT[mes - 1]}`;
}

/* ---------- Helpers de mídia ---------- */
function fotoBackground(pessoa, isHero = false) {
  if (pessoa.foto_url && pessoa.foto_url.trim()) {
    const cls = isHero ? "hero-photo" : "card-photo";
    return `<div class="${cls}" style="background-image:url('${pessoa.foto_url}')"></div>`;
  }
  const inicial = pessoa.nome.charAt(0).toUpperCase();
  const cls = isHero ? "hero-photo" : "card-photo";
  return `<div class="${cls}" style="background:${pickGradient(pessoa.nome)}">
            <div class="placeholder">${inicial}</div>
          </div>`;
}

function shortCountdown(c) {
  if (c.meses > 0)  return `${c.meses} ${c.meses === 1 ? "mês" : "meses"} · ${c.dias}d`;
  if (c.dias  > 0)  return `${c.dias} ${c.dias === 1 ? "dia" : "dias"} · ${c.horas}h`;
  if (c.horas > 0)  return `faltam ${c.horas}h ${c.minutos}m`;
  return `faltam ${c.minutos} min`;
}

/* ---------- Confete ---------- */
function gerarConfete(qtd = 24) {
  let html = '<div class="confetti">';
  const cores = ["#E50914", "#ffffff", "#ff5e69", "#ffd166", "#06d6a0"];
  for (let i = 0; i < qtd; i++) {
    const left   = Math.random() * 100;
    const delay  = (Math.random() * 4).toFixed(2);
    const dur    = (3 + Math.random() * 3).toFixed(2);
    const cor    = cores[Math.floor(Math.random() * cores.length)];
    const rot    = Math.random() * 360;
    html += `<span style="left:${left}%;background:${cor};
      animation-duration:${dur}s;animation-delay:${delay}s;
      transform:rotate(${rot}deg)"></span>`;
  }
  return html + "</div>";
}

/* ---------- Render: Hero ---------- */
function renderHero(pessoa) {
  const { dia, mes } = parseData(pessoa.data);
  const hoje   = isHoje(dia, mes);
  const target = proximoAniversario(dia, mes);
  const c      = calcularContagem(target);
  const hero   = document.getElementById("hero");

  const badge = hoje
    ? `<span class="hero-badge" style="background:#fff;color:var(--red)">🎉 É HOJE!</span>`
    : `<span class="hero-badge">Próximo aniversário</span>`;

  const legenda = pessoa.legenda?.trim()
    ? `<p class="hero-legend">"${pessoa.legenda}"</p>` : "";

  const bloco = hoje
    ? `<div class="celebration">FELIZ ANIVERSÁRIO!</div>
       <p class="hero-date-label">${formatarData(dia, mes)} · um dia especial 🎂</p>`
    : `<p class="hero-date-label">${formatarData(dia, mes)} — faltam:</p>
       <div class="countdown">
         <div class="countdown-unit"><div class="countdown-value">${c.meses}</div>
           <div class="countdown-label">${c.meses === 1 ? "mês" : "meses"}</div></div>
         <div class="countdown-unit"><div class="countdown-value">${c.dias}</div>
           <div class="countdown-label">${c.dias === 1 ? "dia" : "dias"}</div></div>
         <div class="countdown-unit"><div class="countdown-value">${c.horas}</div>
           <div class="countdown-label">${c.horas === 1 ? "hora" : "horas"}</div></div>
         <div class="countdown-unit"><div class="countdown-value">${c.minutos}</div>
           <div class="countdown-label">min</div></div>
       </div>`;

  hero.innerHTML = `
    <div class="hero-card fade-in ${hoje ? "birthday-today" : ""}">
      ${fotoBackground(pessoa, true)}
      <div class="hero-overlay"></div>
      ${hoje ? gerarConfete(30) : ""}
      <div class="hero-content">
        ${badge}
        <h2 class="hero-name">${pessoa.nome}</h2>
        ${legenda}
        ${bloco}
      </div>
    </div>`;
}

/* ---------- Render: Card ---------- */
function renderCard(pessoa) {
  const { dia, mes } = parseData(pessoa.data);
  const hoje   = isHoje(dia, mes);
  const target = proximoAniversario(dia, mes);
  const c      = calcularContagem(target);

  // Barra de progresso: % do ciclo do ano dela já percorrido
  const now = new Date();
  let ultimo = new Date(now.getFullYear(), mes - 1, dia);
  if (ultimo > now) ultimo.setFullYear(ultimo.getFullYear() - 1);
  const proximo = new Date(ultimo);
  proximo.setFullYear(proximo.getFullYear() + 1);
  const pct = Math.max(0, Math.min(100, ((now - ultimo) / (proximo - ultimo)) * 100));

  const txt = hoje ? "🎉 É HOJE!" : shortCountdown(c);

  return `
    <div class="card ${hoje ? "today" : ""}" tabindex="0">
      ${fotoBackground(pessoa)}
      <div class="card-overlay"></div>
      <div class="card-content">
        <div class="card-name">${pessoa.nome}</div>
        <div class="card-date">${formatarData(dia, mes)}</div>
        <div class="card-countdown">${txt}</div>
      </div>
      <div class="card-progress" style="width:${pct}%"></div>
    </div>`;
}

/* ---------- Ordenação ---------- */
function ordenarPorProximidade(lista) {
  return [...lista].sort((a, b) => {
    const da = parseData(a.data);
    const db = parseData(b.data);
    const aHoje = isHoje(da.dia, da.mes);
    const bHoje = isHoje(db.dia, db.mes);
    if (aHoje && !bHoje) return -1;
    if (bHoje && !aHoje) return 1;
    return proximoAniversario(da.dia, da.mes) - proximoAniversario(db.dia, db.mes);
  });
}

/* ---------- Render principal ---------- */
function render() {
  const upcomingEl = document.getElementById("upcoming");
  const scrollX = upcomingEl ? upcomingEl.scrollLeft : 0;

  const ordenados = ordenarPorProximidade(JOVENS);

  // Hero = primeiro da fila (aniversariante de hoje ou próximo)
  renderHero(ordenados[0]);

  // Próximos = todos os outros
  upcomingEl.innerHTML = ordenados.slice(1).map(renderCard).join("");
  upcomingEl.scrollLeft = scrollX;

  // Todos os membros
  document.getElementById("all-members").innerHTML =
    ordenados.map(renderCard).join("");
}

render();
// Atualiza a contagem a cada minuto (renova ano automaticamente quando a data passa)
setInterval(render, 60_000);
