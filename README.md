# Receituário
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Gerador de Prescrição Médica</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --blue: #185FA5;
      --blue-light: #E6F1FB;
      --blue-dark: #0C447C;
      --text: #1a1a1a;
      --text-secondary: #666;
      --border: #e0e0e0;
      --bg: #f4f6fa;
      --white: #ffffff;
      --radius: 8px;
      --radius-lg: 12px;
    }

    body {
      font-family: 'Segoe UI', system-ui, sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      padding: 2rem 1rem;
    }

    .container {
      max-width: 720px;
      margin: 0 auto;
    }

    /* HEADER */
    .app-header {
      display: flex;
      align-items: center;
      gap: 14px;
      margin-bottom: 1.5rem;
    }
    .app-header .logo {
      width: 48px; height: 48px;
      background: var(--blue);
      border-radius: var(--radius-lg);
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0;
    }
    .app-header .logo svg { width: 26px; height: 26px; }
    .app-header h1 { font-size: 20px; font-weight: 600; color: var(--text); }
    .app-header p { font-size: 13px; color: var(--text-secondary); margin-top: 2px; }

    /* CARD */
    .card {
      background: var(--white);
      border-radius: var(--radius-lg);
      border: 1px solid var(--border);
      padding: 1.25rem 1.5rem;
      margin-bottom: 1rem;
    }
    .card-title {
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.07em;
      text-transform: uppercase;
      color: var(--text-secondary);
      margin-bottom: 12px;
    }

    /* GRID */
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
    .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; }
    .mt { margin-top: 12px; }

    /* FIELDS */
    .field { display: flex; flex-direction: column; gap: 4px; }
    .field label { font-size: 12px; color: var(--text-secondary); }
    .field input,
    .field textarea,
    .field select {
      font-family: inherit;
      font-size: 14px;
      color: var(--text);
      background: var(--white);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 8px 10px;
      outline: none;
      transition: border-color 0.15s;
    }
    .field input:focus,
    .field textarea:focus { border-color: var(--blue); box-shadow: 0 0 0 3px rgba(24,95,165,0.1); }
    .field textarea { resize: vertical; min-height: 80px; line-height: 1.5; }

    /* MEDS */
    .med-header {
      display: grid;
      grid-template-columns: 2fr 1fr 1.5fr 36px;
      gap: 8px;
      margin-bottom: 6px;
    }
    .med-header span { font-size: 11px; color: var(--text-secondary); }
    .med-row {
      display: grid;
      grid-template-columns: 2fr 1fr 1.5fr 36px;
      gap: 8px;
      align-items: center;
      margin-bottom: 8px;
    }
    .med-row input {
      font-family: inherit;
      font-size: 14px;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 8px 10px;
      outline: none;
      width: 100%;
      transition: border-color 0.15s;
    }
    .med-row input:focus { border-color: var(--blue); box-shadow: 0 0 0 3px rgba(24,95,165,0.1); }
    .btn-remove {
      width: 36px; height: 36px;
      border: 1px solid var(--border);
      background: #fff;
      border-radius: var(--radius);
      cursor: pointer;
      font-size: 18px;
      color: var(--text-secondary);
      display: flex; align-items: center; justify-content: center;
      transition: all 0.15s;
    }
    .btn-remove:hover { background: #FCEBEB; color: #A32D2D; border-color: #F09595; }

    .btn-add {
      font-size: 13px;
      color: var(--blue);
      background: none;
      border: none;
      cursor: pointer;
      padding: 6px 0;
      display: flex;
      align-items: center;
      gap: 6px;
      font-family: inherit;
    }
    .btn-add:hover { opacity: 0.7; }

    /* ACTIONS */
    .actions {
      display: flex;
      gap: 10px;
      margin-top: 0.5rem;
    }
    .btn-print {
      flex: 1;
      padding: 12px;
      background: var(--blue);
      color: white;
      border: none;
      border-radius: var(--radius);
      font-size: 15px;
      font-weight: 500;
      cursor: pointer;
      font-family: inherit;
      transition: background 0.15s;
    }
    .btn-print:hover { background: var(--blue-dark); }
    .btn-clear {
      padding: 12px 20px;
      background: none;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      font-size: 14px;
      cursor: pointer;
      color: var(--text-secondary);
      font-family: inherit;
    }
    .btn-clear:hover { background: var(--bg); }

    /* FOOTER */
    .app-footer {
      text-align: center;
      font-size: 12px;
      color: #bbb;
      margin-top: 1.5rem;
    }

    /* RESPONSIVE */
    @media (max-width: 520px) {
      .grid-2, .grid-3 { grid-template-columns: 1fr; }
      .med-header { display: none; }
      .med-row { grid-template-columns: 1fr 36px; grid-template-rows: auto auto auto; }
      .med-row input:nth-child(1) { grid-column: 1; }
      .med-row input:nth-child(2) { grid-column: 1; }
      .med-row input:nth-child(3) { grid-column: 1; }
      .btn-remove { grid-row: 1; grid-column: 2; }
    }

    /* PRINT WINDOW STYLES (injected into popup) */
  </style>
</head>
<body>
  <div class="container">

    <div class="app-header">
      <div class="logo">
        <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M19 3H5a2 2 0 00-2 2v14a2 2 0 002 2h14a2 2 0 002-2V5a2 2 0 00-2-2zm-7 3a1 1 0 011 1v2h2a1 1 0 010 2h-2v2a1 1 0 01-2 0v-2H9a1 1 0 010-2h2V7a1 1 0 011-1z" fill="white"/>
        </svg>
      </div>
      <div>
        <h1>Gerador de Prescrição Médica</h1>
        <p>Preencha os dados e clique em imprimir para gerar a prescrição</p>
      </div>
    </div>

    <!-- DADOS DO MÉDICO -->
    <div class="card">
      <div class="card-title">Dados do médico</div>
      <div class="grid-2">
        <div class="field"><label>Nome completo</label><input type="text" id="med-nome" placeholder="Dr. João da Silva" /></div>
        <div class="field"><label>CRM</label><input type="text" id="med-crm" placeholder="CRM/SC 12345" /></div>
      </div>
      <div class="grid-2 mt">
        <div class="field"><label>Especialidade</label><input type="text" id="med-esp" placeholder="Clínica Geral" /></div>
        <div class="field"><label>Telefone / contato</label><input type="text" id="med-tel" placeholder="(48) 99999-0000" /></div>
      </div>
      <div class="mt">
        <div class="field"><label>Endereço do consultório</label><input type="text" id="med-end" placeholder="Rua das Flores, 123 — Centro, Palhoça/SC" /></div>
      </div>
    </div>

    <!-- DADOS DO PACIENTE -->
    <div class="card">
      <div class="card-title">Dados do paciente</div>
      <div class="grid-2">
        <div class="field"><label>Nome do paciente</label><input type="text" id="pac-nome" placeholder="Maria Oliveira" /></div>
        <div class="field"><label>Data de nascimento / idade</label><input type="text" id="pac-idade" placeholder="15/03/1980  (44 anos)" /></div>
      </div>
    </div>

    <!-- MEDICAMENTOS -->
    <div class="card">
      <div class="card-title">Medicamentos</div>
      <div class="med-header">
        <span>Medicamento</span>
        <span>Dosagem</span>
        <span>Posologia</span>
        <span></span>
      </div>
      <div id="meds-container">
        <div class="med-row">
          <input type="text" placeholder="Ex: Amoxicilina 500mg" />
          <input type="text" placeholder="1 cápsula" />
          <input type="text" placeholder="8/8h por 7 dias" />
          <button class="btn-remove" onclick="removeMed(this)" title="Remover">×</button>
        </div>
      </div>
      <button class="btn-add" onclick="addMed()">＋ adicionar medicamento</button>
    </div>

    <!-- ORIENTAÇÕES -->
    <div class="card">
      <div class="card-title">Orientações / observações</div>
      <div class="field">
        <textarea id="obs" placeholder="Ex: Tomar com alimentos. Evitar exposição ao sol. Retornar em 15 dias se não houver melhora."></textarea>
      </div>
    </div>

    <!-- AÇÕES -->
    <div class="actions">
      <button class="btn-clear" onclick="limpar()">Limpar tudo</button>
      <button class="btn-print" onclick="imprimir()">🖨️ Imprimir prescrição</button>
    </div>

    <div class="app-footer">
      Site gerado via GitHub Pages &mdash; uso interno
    </div>

  </div>

  <script>
    function addMed() {
      const c = document.getElementById('meds-container');
      const row = document.createElement('div');
      row.className = 'med-row';
      row.innerHTML = `
        <input type="text" placeholder="Ex: Dipirona 500mg" />
        <input type="text" placeholder="1 comprimido" />
        <input type="text" placeholder="6/6h se dor" />
        <button class="btn-remove" onclick="removeMed(this)" title="Remover">×</button>
      `;
      c.appendChild(row);
    }

    function removeMed(btn) {
      const rows = document.querySelectorAll('.med-row');
      if (rows.length > 1) btn.closest('.med-row').remove();
    }

    function limpar() {
      if (confirm('Deseja apagar todos os campos?')) {
        document.querySelectorAll('input, textarea').forEach(el => el.value = '');
      }
    }

    function imprimir() {
      const nome    = document.getElementById('med-nome').value.trim() || 'Médico';
      const crm     = document.getElementById('med-crm').value.trim();
      const esp     = document.getElementById('med-esp').value.trim();
      const tel     = document.getElementById('med-tel').value.trim();
      const end     = document.getElementById('med-end').value.trim();
      const pacNome = document.getElementById('pac-nome').value.trim() || 'Paciente';
      const pacIdade= document.getElementById('pac-idade').value.trim();
      const obs     = document.getElementById('obs').value.trim();
      const hoje    = new Date().toLocaleDateString('pt-BR');

      let medsHtml = '';
      document.querySelectorAll('.med-row').forEach((row, i) => {
        const inputs = row.querySelectorAll('input');
        const med  = inputs[0] ? inputs[0].value.trim() : '';
        const dose = inputs[1] ? inputs[1].value.trim() : '';
        const pos  = inputs[2] ? inputs[2].value.trim() : '';
        if (med) {
          medsHtml += `
            <div style="margin-bottom:14px;padding-left:16px;border-left:3px solid #185FA5;">
              <div style="font-size:15px;font-weight:600;color:#1a1a1a;">${i+1}. ${med}</div>
              ${dose ? `<div style="font-size:13px;color:#555;margin-top:2px;">Dose: ${dose}</div>` : ''}
              ${pos  ? `<div style="font-size:13px;color:#555;">Posologia: ${pos}</div>` : ''}
            </div>`;
        }
      });

      if (!medsHtml) {
        medsHtml = '<p style="color:#aaa;font-size:13px;">Nenhum medicamento informado.</p>';
      }

      const printHtml = `<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8"/>
  <title>Prescrição — ${pacNome}</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Segoe UI', Georgia, serif; background: #f0f0f0; padding: 24px; }
    .page {
      background: white;
      max-width: 680px;
      margin: 0 auto;
      padding: 40px 44px 60px;
      min-height: 950px;
      position: relative;
      border: 1px solid #ccc;
    }
    .doc-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      padding-bottom: 16px;
      border-bottom: 2.5px solid #185FA5;
      margin-bottom: 28px;
    }
    .doc-header .med-info h2 { font-size: 20px; color: #185FA5; font-weight: 700; }
    .doc-header .med-info p  { font-size: 13px; color: #666; margin-top: 2px; }
    .doc-header .contact     { text-align: right; font-size: 12px; color: #888; line-height: 1.6; max-width: 200px; }
    .rx-title {
      text-align: center;
      font-size: 17px;
      font-weight: 700;
      letter-spacing: 0.2em;
      color: #222;
      margin-bottom: 22px;
    }
    .pac-bar {
      display: flex;
      gap: 16px;
      flex-wrap: wrap;
      background: #f7f9fc;
      border-radius: 6px;
      padding: 10px 14px;
      font-size: 13px;
      margin-bottom: 26px;
      border: 1px solid #e4eaf3;
    }
    .pac-bar .label { color: #888; }
    .pac-bar .val   { font-weight: 600; }
    .pac-bar .date  { margin-left: auto; }
    .section-label {
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      color: #aaa;
      margin-bottom: 14px;
    }
    .obs-box {
      margin-top: 28px;
      border: 1px solid #e0e0e0;
      border-radius: 6px;
      padding: 14px 16px;
    }
    .obs-box .section-label { margin-bottom: 8px; }
    .obs-box p { font-size: 13px; line-height: 1.7; color: #333; }
    .signature {
      position: absolute;
      bottom: 40px;
      right: 44px;
      text-align: center;
    }
    .signature .line {
      border-top: 1px solid #333;
      width: 200px;
      margin-bottom: 6px;
    }
    .signature .sig-name { font-size: 13px; font-weight: 600; }
    .signature .sig-crm  { font-size: 11px; color: #777; margin-top: 2px; }
    .print-date {
      position: absolute;
      bottom: 44px;
      left: 44px;
      font-size: 11px;
      color: #bbb;
    }
    @media print {
      body { background: white; padding: 0; }
      .page { border: none; max-width: 100%; min-height: auto; }
    }
  </style>
</head>
<body>
  <div class="page">
    <div class="doc-header">
      <div class="med-info">
        <h2>${nome}</h2>
        ${esp ? `<p>${esp}</p>` : ''}
        ${crm ? `<p>${crm}</p>` : ''}
      </div>
      <div class="contact">
        ${tel ? `<div>${tel}</div>` : ''}
        ${end ? `<div>${end}</div>` : ''}
      </div>
    </div>

    <div class="rx-title">RECEITUÁRIO MÉDICO</div>

    <div class="pac-bar">
      <div><span class="label">Paciente: </span><span class="val">${pacNome}</span></div>
      ${pacIdade ? `<div><span class="label">Nasc./Idade: </span><span class="val">${pacIdade}</span></div>` : ''}
      <div class="date"><span class="label">Data: </span><span class="val">${hoje}</span></div>
    </div>

    <div class="section-label">Prescrição</div>
    ${medsHtml}

    ${obs ? `<div class="obs-box">
      <div class="section-label">Orientações</div>
      <p>${obs.replace(/\n/g, '<br/>')}</p>
    </div>` : ''}

    <div class="print-date">${hoje}</div>
    <div class="signature">
      <div class="line"></div>
      <div class="sig-name">${nome}</div>
      ${crm ? `<div class="sig-crm">${crm}</div>` : ''}
    </div>
  </div>
  <script>window.onload = function(){ window.print(); }<\/script>
</body>
</html>`;

      const win = window.open('', '_blank', 'width=800,height=950');
      if (win) {
        win.document.write(printHtml);
        win.document.close();
      } else {
        alert('Permita pop-ups para este site para gerar a prescrição.');
      }
    }
  </script>
</body>
</html>
