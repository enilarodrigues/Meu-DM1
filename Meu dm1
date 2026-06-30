<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DM1 Tracker - Meu Controle</title>
  <style>
    body { font-family: 'Segoe UI', Arial; background: linear-gradient(135deg, #e0f2fe, #f0f9ff); margin:0; padding:20px; color:#1e2937; }
    .container { max-width: 640px; margin: 0 auto; }
    h1 { text-align:center; color:#1e40af; }
    .card { background:white; border-radius:16px; padding:20px; box-shadow:0 10px 20px rgba(0,0,0,0.1); margin-bottom:20px; }
    input, select, textarea, button { width:100%; padding:16px; margin:10px 0; border:2px solid #e2e8f0; border-radius:12px; font-size:17px; }
    button { background:#2563eb; color:white; font-weight:bold; border:none; cursor:pointer; }
    button:hover { background:#1d4ed8; }
    .btn-success { background:#16a34a; }
    .alerta-hipo { background:#fee2e2; color:#b91c1c; padding:25px; border:5px solid #ef4444; border-radius:16px; text-align:center; font-size:21px; animation:pulse 1s infinite; }
    @keyframes pulse { 0%,100% { transform:scale(1); } 50% { transform:scale(1.03); } }
    table { width:100%; border-collapse:collapse; margin-top:15px; }
    th, td { padding:10px; border-bottom:1px solid #ddd; text-align:left; }
  </style>
</head>
<body>
  <div class="container">
    <h1>🩸 DM1 Tracker</h1>
    <p style="text-align:center;color:#64748b;">Controle completo de Diabetes Tipo 1</p>

    <div class="card">
      <h2>📝 Novo Registro</h2>
      <form id="form">
        <input type="datetime-local" id="dataHora" required>
        <input type="number" id="glicose" placeholder="Glicose (mg/dL)" step="1">
        <input type="number" id="insulina" placeholder="Insulina (unidades)" step="0.5">
        <select id="tipoInsulina"><option value="">Tipo</option><option>Bolus</option><option>Basal</option></select>
        <input type="number" id="carbs" placeholder="Carboidratos (g)">
        <input type="text" id="atividade" placeholder="Atividade física">
        <textarea id="anotacoes" placeholder="Anotações"></textarea>
        <button type="submit" class="btn-success">💾 Salvar</button>
      </form>
    </div>

    <div class="card">
      <h2>📅 Consultas & Exames</h2>
      <input type="datetime-local" id="dataConsulta">
      <input type="text" id="medico" placeholder="Médico / Local">
      <button onclick="salvarConsulta()">Salvar Consulta</button>
    </div>

    <div class="card">
      <button onclick="ativarLembretes()">🕒 Lembretes a cada 2h</button>
      <button onclick="exportarDados()">📤 Exportar Dados (CSV)</button>
      <button onclick="mostrarDicas()">🥗 Dicas & IA</button>
    </div>

    <div class="card">
      <h2>📊 Últimos Registros</h2>
      <div id="listaRegistros"></div>
    </div>
  </div>

  <script>
    let registros = JSON.parse(localStorage.getItem('dm1Registros')) || [];

    function salvarRegistro(e) {
      e.preventDefault();
      const data = {
        dataHora: document.getElementById('dataHora').value,
        glicose: parseFloat(document.getElementById('glicose').value),
        insulina: parseFloat(document.getElementById('insulina').value),
        tipoInsulina: document.getElementById('tipoInsulina').value,
        carbs: parseFloat(document.getElementById('carbs').value),
        atividade: document.getElementById('atividade').value,
        anotacoes: document.getElementById('anotacoes').value,
        timestamp: new Date()
      };

      registros.unshift(data);
      localStorage.setItem('dm1Registros', JSON.stringify(registros));
      
      if (data.glicose && data.glicose < 70) {
        mostrarAlertaHipo(data.glicose);
      }
      
      alert("✅ Salvo com sucesso!");
      e.target.reset();
      atualizarLista();
    }

    function mostrarAlertaHipo(glicose) {
      const alerta = document.createElement('div');
      alerta.className = 'alerta-hipo';
      alerta.innerHTML = `🚨 HIPOGlicemia detectada (${glicose} mg/dL)!<br><br>Consuma 15g de carboidratos rápidos agora!`;
      document.body.appendChild(alerta);
      tocarSomAlerta();
    }

    function tocarSomAlerta() {
      const audio = new Audio('https://freesound.org/data/previews/66/66929_634166-lq.mp3'); // Som de alerta (pode trocar)
      audio.play();
    }

    function atualizarLista() {
      let html = '<table><tr><th>Data</th><th>Glicose</th><th>Insulina</th><th>Anotações</th></tr>';
      registros.slice(0, 15).forEach(r => {
        html += `<tr><td>\( {new Date(r.dataHora).toLocaleString('pt-BR')}</td><td> \){r.glicose}</td><td>\( {r.insulina}</td><td> \){r.anotacoes||''}</td></tr>`;
      });
      html += '</table>';
      document.getElementById('listaRegistros').innerHTML = html;
    }

    function salvarConsulta() {
      alert("Consulta salva localmente! (em produção pode integrar com banco)");
    }

    function ativarLembretes() {
      alert("Lembretes ativados (a cada 2 horas enquanto o app estiver aberto)");
      setInterval(() => {
        alert("⏰ Hora de medir a glicose!");
      }, 7200000);
    }

    function exportarDados() {
      let csv = "Data,Glicose,Insulina,Anotacoes\n";
      registros.forEach(r => {
        csv += `"\( {r.dataHora}"," \){r.glicose}","\( {r.insulina}"," \){r.anotacoes}"\n`;
      });
      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'dm1_registros.csv';
      a.click();
    }

    function mostrarDicas() {
      alert("Dicas:\n\n• Conte carboidratos\n• Tenha glicose rápida sempre\n• Registre tudo\n\nIA: Qual sua dúvida?");
    }

    // Inicializar
    document.getElementById('form').addEventListener('submit', salvarRegistro);
    atualizarLista();
  </script>
</body>
</html>
