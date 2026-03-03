<!DOCTYPE html>
<html lang="pt-pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard de Performance - Shopee Expedição</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        :root {
            --shopee-orange: #EE4D2D;
            --shopee-bg: #1a1a1a;
            --card-bg: #2d2d2d;
            --text-light: #ffffff;
            --success: #22c55e;
            --danger: #ef4444;
            --gold: #ffd700;
            --silver: #c0c0c0;
            --bronze: #cd7f32;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--shopee-bg);
            color: var(--text-light);
            margin: 0;
            padding: 10px 20px;
            overflow: hidden;
        }

        /* Navegação */
        .nav-tabs { display: flex; gap: 10px; margin-bottom: 10px; }
        .tab-btn {
            background: #444; color: white; border: none; padding: 8px 15px;
            border-radius: 5px; cursor: pointer; font-size: 14px; font-weight: 600;
        }
        .tab-btn.active { background: var(--shopee-orange); }
        .tab-content { display: none; }
        .tab-content.active { display: block; }

        /* Cabeçalho */
        .header-tv {
            display: flex; justify-content: space-between; align-items: center;
            background: var(--shopee-orange); padding: 15px 30px; border-radius: 10px;
            margin-bottom: 15px; box-shadow: 0 4px 15px rgba(0,0,0,0.5);
        }
        .header-left h1 { margin: 0; font-size: 28px; font-weight: 700; text-transform: uppercase; letter-spacing: 1px; }
        .clock { font-size: 26px; font-weight: 700; }
        .shopee-logo { height: 40px; filter: brightness(0) invert(1); }

        /* KPIs */
        .kpi-container {
            display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-bottom: 10px;
        }
        .kpi-card {
            background: var(--card-bg); padding: 10px; border-radius: 8px;
            text-align: center; border-bottom: 3px solid var(--shopee-orange);
        }
        .kpi-card .label { font-size: 11px; color: #aaa; text-transform: uppercase; }
        .kpi-card .value { font-size: 24px; font-weight: 700; color: var(--shopee-orange); }

        /* Alerta Líder */
        .leader-alert {
            background: linear-gradient(45deg, #EE4D2D, #ff8c00);
            padding: 12px 20px; border-radius: 10px; margin-bottom: 10px;
            display: flex; align-items: center; gap: 15px; animation: pulse 2s infinite;
        }

        /* Layout Principal */
        .main-content {
            display: grid; grid-template-columns: 1.5fr 1fr; gap: 15px;
            height: calc(100vh - 240px);
        }

        .ranking-table-container {
            background: var(--card-bg); border-radius: 10px; padding: 15px;
            overflow-y: auto; box-shadow: 0 4px 10px rgba(0,0,0,0.2);
        }

        table { width: 100%; border-collapse: collapse; }
        th { text-align: left; font-size: 12px; color: #888; padding: 10px; border-bottom: 2px solid #444; }
        td { padding: 8px 10px; font-size: 14px; border-bottom: 1px solid #3d3d3d; }

        .rank-gold { color: var(--gold); font-weight: 800; font-size: 18px; }
        .rank-silver { color: var(--silver); font-weight: 800; font-size: 17px; }
        .rank-bronze { color: var(--bronze); font-weight: 800; font-size: 16px; }
        .row-gold { background: rgba(255, 215, 0, 0.08) !important; }
        .row-silver { background: rgba(192, 192, 192, 0.05) !important; }
        .row-bronze { background: rgba(205, 127, 50, 0.05) !important; }

        .tag-sla { padding: 2px 8px; border-radius: 10px; font-size: 11px; font-weight: 700; }
        .bg-success { background: rgba(34, 197, 94, 0.2); color: var(--success); border: 1px solid var(--success); }
        .bg-danger { background: rgba(239, 68, 68, 0.2); color: var(--danger); border: 1px solid var(--danger); }

        /* ADMIN - DESIGN LIMPO */
        .admin-panel { background: #fff; color: #333; padding: 25px; border-radius: 10px; box-shadow: 0 5px 20px rgba(0,0,0,0.3); }
        .admin-section { margin-bottom: 25px; border-bottom: 1px solid #eee; padding-bottom: 15px; }
        .admin-section h3 { margin-top: 0; color: #444; font-size: 16px; display: flex; align-items: center; gap: 8px; }
        .admin-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; }
        .admin-field { display: flex; flex-direction: column; gap: 5px; }
        .admin-field label { font-size: 12px; font-weight: 700; color: #666; text-transform: uppercase; }
        .admin-field select, .admin-field input { 
            padding: 10px; border: 1px solid #ddd; border-radius: 5px; font-family: inherit; font-size: 14px;
        }
        .btn-update { background: var(--shopee-orange); color: white; border: none; padding: 14px; width: 100%; border-radius: 5px; font-weight: 700; cursor: pointer; transition: 0.2s; font-size: 16px; }
        
        .import-box {
            border: 2px dashed var(--shopee-orange); text-align: center; border-radius: 10px; background: #fff4f2; padding: 20px;
            margin-top: 10px;
        }

        .summary-box {
            background: #363636; border-radius: 8px; padding: 12px; margin-bottom: 10px;
            border-left: 4px solid var(--shopee-orange);
        }
        .summary-item { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 13px; }
        .summary-item b { color: var(--shopee-orange); }

        @keyframes pulse { 0% { opacity: 0.9; } 50% { opacity: 1; } 100% { opacity: 0.9; } }
    </style>
</head>
<body>

    <div class="nav-tabs">
        <button class="tab-btn active" onclick="openTab('dashboard')"><i class="fa-solid fa-chart-line"></i> Dashboard</button>
        <button class="tab-btn" onclick="openTab('admin')"><i class="fa-solid fa-gears"></i> Admin / Config</button>
    </div>

    <!-- ABA DASHBOARD -->
    <div id="dashboard" class="tab-content active">
        <div class="header-tv">
            <div class="header-left">
                <h1 id="header-shift-title">EXPEDIÇÃO | TURNO --</h1>
            </div>
            <div class="header-right" style="display: flex; align-items: center; gap: 30px;">
                <div class="clock" id="clock-display">00:00:00</div>
                <img src="https://upload.wikimedia.org/wikipedia/commons/f/fe/Shopee.svg" class="shopee-logo">
            </div>
        </div>

        <div class="kpi-container">
            <div class="kpi-card"><div class="label">Média do Turno</div><div class="value" id="avg-day">0,00m</div></div>
            <div class="kpi-card"><div class="label">Aderência SLA (Total)</div><div class="value" id="sla-perc">0%</div></div>
            <div class="kpi-card"><div class="label">Bancadas Ativas</div><div class="value" id="kpi-stations">0</div></div>
            <div class="kpi-card"><div class="label">Total Operadores</div><div class="value" id="kpi-staff">0</div></div>
        </div>

        <div class="leader-alert">
            <i class="fa-solid fa-crown" style="font-size: 30px; color: var(--gold);"></i>
            <div>
                <h2 style="margin: 0; font-size: 13px; opacity: 0.9; text-transform: uppercase;">Líder de Produtividade</h2>
                <p style="margin: 0; font-size: 18px; font-weight: 700; color: white;" id="top-winner-text">Aguardando dados...</p>
            </div>
        </div>

        <div class="main-content">
            <div class="ranking-table-container">
                <table id="ranking-table">
                    <thead>
                        <tr>
                            <th width="70">Pos.</th>
                            <th>Operador</th>
                            <th width="80">Bancada</th>
                            <th width="90">Média</th>
                            <th width="80">Status</th>
                        </tr>
                    </thead>
                    <tbody id="ranking-body"></tbody>
                </table>
            </div>

            <div style="display: flex; flex-direction: column; gap: 10px;">
                <div class="ranking-table-container">
                    <h3 style="margin-top:0; font-size: 16px; color: var(--shopee-orange); border-bottom: 1px solid #444; padding-bottom: 5px;">
                        <i class="fa-solid fa-circle-info"></i> Informações do Turno
                    </h3>
                    <div class="summary-box">
                        <div class="summary-item"><span>Período Ativo:</span> <b id="sum-shift-type">--</b></div>
                        <div class="summary-item"><span>Bancadas Configuradas:</span> <b id="sum-stations">0</b></div>
                        <div class="summary-item"><span>Operadores em Campo:</span> <b id="sum-staff">0</b></div>
                        <div class="summary-item"><span>Eficiência Geral:</span> <b id="sum-eff">0%</b></div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- ABA ADMIN / CONFIG -->
    <div id="admin" class="tab-content">
        <div class="admin-panel">
            <div class="admin-section">
                <h3><i class="fa-solid fa-sliders"></i> Ajustes de Operação</h3>
                <div class="admin-grid">
                    <div class="admin-field">
                        <label>Turno (AM ou PM)</label>
                        <select id="input-shift">
                            <option value="AM">AM (Manhã)</option>
                            <option value="PM">PM (Tarde/Noite)</option>
                        </select>
                    </div>
                    <div class="admin-field">
                        <label>Quantidade de Bancadas</label>
                        <input type="number" id="input-stations" value="15">
                    </div>
                    <div class="admin-field">
                        <label>Quantidade de Operadores</label>
                        <input type="number" id="input-staff-count" value="15" placeholder="Ex: 30 para duplas">
                    </div>
                    <div class="admin-field">
                        <label>Meta SLA (minutos)</label>
                        <input type="number" id="input-goal" value="7">
                    </div>
                </div>
            </div>

            <div class="admin-section">
                <h3><i class="fa-solid fa-file-import"></i> Atualizar Dados do Turno</h3>
                <div class="import-box">
                    <input type="file" id="file-input" accept=".csv" style="display:none" onchange="handleFileUpload(event)">
                    <button class="tab-btn" style="background: var(--shopee-orange); padding: 12px 25px;" onclick="document.getElementById('file-input').click()">
                        <i class="fa-solid fa-upload"></i> CARREGAR CSV DO SISTEMA
                    </button>
                    <p style="margin-top: 10px; font-size: 12px; color: #666;">Os dados importados substituirão a lista atual do ranking.</p>
                </div>
            </div>

            <button class="btn-update" onclick="render(); openTab('dashboard');">
                <i class="fa-solid fa-check-double"></i> APLICAR E ATUALIZAR DASHBOARD
            </button>
        </div>
    </div>

    <script>
        // Dados de exemplo
        let database = [
            { nome: "Operador Exemplo 1", bancada: "A-01", tempo: 5.20 },
            { nome: "Operador Exemplo 2", bancada: "A-02", tempo: 8.45 }
        ];

        function openTab(name) {
            document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(name).classList.add('active');
            
            const btns = document.querySelectorAll('.tab-btn');
            if(name === 'dashboard') btns[0].classList.add('active');
            else btns[1].classList.add('active');
        }

        function handleFileUpload(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = e => parseCSV(e.target.result);
            reader.readAsText(file);
        }

        function parseCSV(text) {
            const lines = text.split(/\r?\n/).filter(l => l.trim().length > 0);
            const delimiter = lines[0].includes(';') ? ';' : ',';
            const headers = lines[0].split(delimiter).map(h => h.trim().replace(/"/g, ''));
            
            const idxOp = headers.findIndex(h => h.toLowerCase().includes('operator'));
            const idxBancada = headers.findIndex(h => h.toLowerCase().includes('cage') || h.toLowerCase().includes('corridor'));
            const idxStart = headers.findIndex(h => h.toLowerCase().includes('start'));
            const idxEnd = headers.findIndex(h => h.toLowerCase().includes('end'));

            let newList = [];
            for (let i = 1; i < lines.length; i++) {
                const cols = lines[i].split(delimiter).map(c => c.trim().replace(/"/g, ''));
                if(cols.length < 2) continue;
                
                let nome = (cols[idxOp] || "").replace(/\[.*?\]/g, '').trim();
                let bancada = cols[idxBancada] || "N/A";
                let tempo = 0;

                if (idxStart !== -1 && idxEnd !== -1 && cols[idxStart] && cols[idxEnd]) {
                    const t1 = new Date(cols[idxStart].replace('\n', ' '));
                    const t2 = new Date(cols[idxEnd].split('[')[0].trim());
                    tempo = Math.abs(t2 - t1) / (1000 * 60);
                }

                if (nome && tempo > 0) newList.push({ nome, bancada, tempo });
            }
            if (newList.length > 0) { 
                database = newList; 
                render(); 
                openTab('dashboard'); 
            }
        }

        function render() {
            const body = document.getElementById('ranking-body');
            body.innerHTML = '';
            database.sort((a, b) => a.tempo - b.tempo);

            const meta = parseFloat(document.getElementById('input-goal').value) || 7;
            const turno = document.getElementById('input-shift').value;
            const bancadasConfig = document.getElementById('input-stations').value;
            const operadoresConfig = document.getElementById('input-staff-count').value;
            
            // Atualiza Textos
            document.getElementById('header-shift-title').innerText = `EXPEDIÇÃO | TURNO ${turno}`;
            document.getElementById('kpi-stations').innerText = bancadasConfig;
            document.getElementById('kpi-staff').innerText = operadoresConfig;
            document.getElementById('sum-shift-type').innerText = turno;
            document.getElementById('sum-stations').innerText = bancadasConfig;
            document.getElementById('sum-staff').innerText = operadoresConfig;

            database.forEach((u, i) => {
                const pos = i + 1;
                const ok = u.tempo <= meta;
                
                let medalIcon = "";
                let rankClass = "";
                let rowClass = "";

                if(pos === 1) { medalIcon = '🥇 '; rankClass = "rank-gold"; rowClass = "row-gold"; }
                else if(pos === 2) { medalIcon = '🥈 '; rankClass = "rank-silver"; rowClass = "row-silver"; }
                else if(pos === 3) { medalIcon = '🥉 '; rankClass = "rank-bronze"; rowClass = "row-bronze"; }

                body.innerHTML += `
                    <tr class="${rowClass}">
                        <td class="${rankClass}">${medalIcon}${pos}º</td>
                        <td style="font-weight:${pos <= 10 ? '700' : '400'}">${u.nome}</td>
                        <td>${u.bancada}</td>
                        <td style="font-weight:700">${u.tempo.toFixed(2)}m</td>
                        <td><span class="tag-sla ${ok ? 'bg-success' : 'bg-danger'}">${ok ? 'META' : 'ALTO'}</span></td>
                    </tr>
                `;
            });

            if(database.length > 0) {
                const top = database[0];
                const avg = database.reduce((a, b) => a + b.tempo, 0) / database.length;
                const slaCount = database.filter(u => u.tempo <= meta).length;
                const slaPerc = (slaCount / database.length) * 100;

                document.getElementById('top-winner-text').innerText = `${top.nome} (${top.bancada}) - ${top.tempo.toFixed(2)}m`;
                document.getElementById('avg-day').innerText = avg.toFixed(2) + 'm';
                document.getElementById('sla-perc').innerText = Math.round(slaPerc) + '%';
                document.getElementById('sum-eff').innerText = Math.round(slaPerc) + '%';
            }
        }

        function updateClock() {
            const now = new Date();
            document.getElementById('clock-display').innerText = now.toLocaleTimeString('pt-PT');
        }

        setInterval(updateClock, 1000);
        updateClock();
        render();
    </script>
</body>
</html>
