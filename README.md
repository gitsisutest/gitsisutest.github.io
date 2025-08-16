<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador SISU Web - UFS</title>
    <link rel="shortcut icon" href="sisujpg.jpg" type="image/x-icon">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --primary-color: #007bff; /* Azul primário */
            --primary-hover-color: #0056b3;
            --secondary-color: #6c757d; /* Cinza secundário */
            --secondary-hover-color: #545b62;
            --success-color: #28a745; /* Verde sucesso */
            --success-hover-color: #1e7e34;
            --danger-color: #dc3545; /* Vermelho perigo */
            --danger-hover-color: #b02a37;
            --warning-color: #ffc107; /* Amarelo aviso */
            --info-color: #17a2b8; /* Azul informação */
            --light-bg: #f8f9fa; /* Fundo claro */
            --dark-text: #343a40; /* Texto escuro */
            --light-text: #ffffff;
            --border-color: #dee2e6;
            --card-bg: #ffffff;
            --shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
            --border-radius: 0.3rem;
            --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            font-family: var(--font-family);
            background-color: var(--light-bg);
            color: var(--dark-text);
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            line-height: 1.6;
        }

        .container {
            width: 100%;
            max-width: 1300px;
            background-color: var(--card-bg);
            padding: 20px;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
        }

        header {
            text-align: center;
            margin-bottom: 25px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 15px;
        }

        header h1 {
            color: var(--primary-color);
            margin: 0;
        }

        .main-layout {
            display: grid;
            grid-template-columns: 450px 1fr;
            gap: 20px;
        }

        .left-column, .right-column {
            padding: 15px;
            background-color: #fdfdfd;
            border-radius: var(--border-radius);
            border: 1px solid var(--border-color);
        }
        
        .left-column {
             max-height: calc(100vh - 100px);
             overflow-y: auto;
        }


        .form-section {
            margin-bottom: 20px;
            padding: 15px;
            border: 1px solid #e0e0e0;
            border-radius: var(--border-radius);
            background-color: #fff;
        }

        .form-section h2 {
            font-size: 1.3em;
            color: var(--primary-color);
            margin-top: 0;
            margin-bottom: 15px;
            padding-bottom: 8px;
            border-bottom: 1px solid var(--border-color);
        }

        .input-group {
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .input-group label {
            font-weight: bold;
            color: var(--dark-text);
            width: 100px;
            text-align: right;
            font-size: 0.9em;
        }

        .input-group input[type="text"],
        .input-group input[type="number"] {
            flex-grow: 1;
            padding: 8px 10px;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            font-size: 0.95em;
            transition: border-color 0.2s;
        }
        .input-group input[type="text"]:focus,
        .input-group input[type="number"]:focus {
            border-color: var(--primary-color);
            outline: none;
            box-shadow: 0 0 0 0.2rem rgba(0,123,255,.25);
        }
        
        .acertos-input {
            width: 50px !important; /* Largura menor para acertos */
            flex-grow: 0 !important;
            text-align: center;
        }

        .tri-display {
            font-size: 0.8em;
            color: var(--secondary-color);
            margin-left: 5px;
            white-space: nowrap;
        }
        
        .total-acertos-graficos {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top:10px;
        }

        #totalAcertosDisplay {
            font-weight: bold;
        }

        .button-group {
            margin-top: 15px;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        .notas-finales-input-group {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-bottom: 12px;
        }
        .notas-finales-input-group label {
             width: 100px;
             text-align: right;
             font-weight: bold;
             font-size: 0.9em;
        }
        .notas-finales-input-group input {
            width: 80px !important;
            flex-grow: 0 !important;
            text-align: center;
        }
        .copy-buttons-container {
            display: flex;
            flex-direction: column;
            gap: 5px;
            margin-left: 15px;
        }

        button {
            padding: 10px 15px;
            border: none;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 0.95em;
            font-weight: bold;
            transition: background-color 0.2s, transform 0.1s;
            background-color: var(--primary-color);
            color: var(--light-text);
        }
        button:hover {
            background-color: var(--primary-hover-color);
            transform: translateY(-1px);
        }
        button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
            transform: none;
        }

        button.secondary { background-color: var(--secondary-color); }
        button.secondary:hover { background-color: var(--secondary-hover-color); }
        button.success { background-color: var(--success-color); }
        button.success:hover { background-color: var(--success-hover-color); }
        button.danger { background-color: var(--danger-color); }
        button.danger:hover { background-color: var(--danger-hover-color); }
        button.info { background-color: var(--info-color); }
        button.info:hover { filter: brightness(110%); }
        
        .btn-sm {
            padding: 5px 10px;
            font-size: 0.8em;
        }

        #cursosList {
            width: 100%;
            height: 150px;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            padding: 5px;
            overflow-y: auto;
            background-color: #fff;
        }
        #cursosList .curso-item {
            padding: 8px;
            cursor: pointer;
            border-bottom: 1px solid #f0f0f0;
            transition: background-color 0.2s;
            font-size: 0.9em;
        }
        #cursosList .curso-item:last-child {
            border-bottom: none;
        }
        #cursosList .curso-item:hover {
            background-color: #e9ecef;
        }
        #cursosList .curso-item.selected {
            background-color: var(--primary-color);
            color: var(--light-text);
            font-weight: bold;
        }

        #mediaSimplesDisplay {
            font-weight: bold;
            margin-top: 10px;
            font-size: 0.95em;
        }

        /* Tabela de Resultados Ao Vivo */
        .live-results-table-container {
            max-height: calc(100vh - 150px); /* Ajuste conforme necessário */
            overflow-y: auto;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
        }
        #liveResultsTable {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85em;
        }
        #liveResultsTable th, #liveResultsTable td {
            border: 1px solid var(--border-color);
            padding: 8px 10px;
            text-align: left;
        }
        #liveResultsTable th {
            background-color: var(--primary-color);
            color: var(--light-text);
            cursor: pointer;
            position: sticky;
            top: 0;
            z-index: 1;
        }
        #liveResultsTable th:hover {
            background-color: var(--primary-hover-color);
        }
        #liveResultsTable tr:nth-child(even) {
            background-color: #f8f9fa;
        }
        #liveResultsTable tr:hover {
            background-color: #e9ecef;
        }
        .status-aprovado { color: var(--success-color); font-weight: bold; }
        .status-excedente { color: var(--danger-color); font-weight: bold; }
        .status-invalido { color: var(--warning-color); font-style: italic; }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.5);
            align-items: center;
            justify-content: center;
        }
        .modal-content {
            background-color: var(--card-bg);
            margin: auto;
            padding: 25px;
            border: 1px solid var(--border-color);
            border-radius: var(--border-radius);
            width: 90%;
            max-width: 500px; /* Default for error/result */
            box-shadow: var(--shadow);
            animation: fadeInModal 0.3s ease-out;
        }
        .modal-content.graficos-modal-content {
             max-width: 900px; /* Larger for charts */
        }

        @keyframes fadeInModal {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 10px;
            margin-bottom: 15px;
        }
        .modal-header h2 {
            margin: 0;
            font-size: 1.5em;
            color: var(--primary-color);
        }
        .modal-header .close-button {
            color: var(--secondary-color);
            font-size: 1.8em;
            font-weight: bold;
            cursor: pointer;
            background: none;
            border: none;
            padding: 0;
        }
        .modal-header .close-button:hover {
            color: var(--dark-text);
        }
        .modal-body p {
            margin-bottom: 10px;
        }
        .modal-body strong { color: var(--primary-color); }
        
        /* Estilos específicos para a tabela de resultados no modal */
        #resultadoVerificacaoTable {
            width: 100%;
            margin-top: 15px;
            border-collapse: collapse;
        }
        #resultadoVerificacaoTable th, #resultadoVerificacaoTable td {
            border: 1px solid var(--border-color);
            padding: 8px;
            text-align: center;
        }
        #resultadoVerificacaoTable th {
            background-color: #f0f0f0;
        }

        /* Responsividade */
        @media (max-width: 992px) {
            .main-layout {
                grid-template-columns: 1fr; /* Coluna única em telas menores */
            }
            .left-column, .right-column {
                max-height: none; /* Remove max-height para permitir rolagem natural */
            }
        }
        @media (max-width: 768px) {
            body { padding: 10px; }
            .container { padding: 15px; }
            .input-group { flex-direction: column; align-items: flex-start; }
            .input-group label { width: auto; text-align: left; margin-bottom: 3px; }
            .input-group input[type="text"], .input-group input[type="number"] { width: 100%; }
            .acertos-input { width: 100% !important; }
            .notas-finales-input-group { flex-direction: column; align-items: flex-start;}
            .notas-finales-input-group input { width: 100% !important; }
            .copy-buttons-container { margin-left:0; margin-top: 10px; width: 100%;}
            .copy-buttons-container button { width: 100%;}
        }

    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Simulador SISU Web - UFS</h1>
        </header>

        <div class="main-layout">
            <div class="left-column">
                <div class="form-section">
                    <h2>Acertos ENEM (0-45)</h2>
                    <div id="acertosEnemContainer">
                        </div>
                    <div class="total-acertos-graficos">
                        <span id="totalAcertosDisplay">Total: 00/180</span>
                        <button id="btnGraficos" class="btn-sm info">Gráficos TRI</button>
                    </div>
                </div>

                <div class="form-section">
                    <h2>Notas Finais ENEM (0-1000)</h2>
                    <div id="notasEnemContainer">
                        </div>
                     <div class="copy-buttons-container">
                        <button id="btnCopiarMinimas" class="btn-sm secondary">Copiar Notas Mínimas (TRI)</button>
                        <button id="btnCopiarMedias" class="btn-sm secondary">Copiar Notas Médias (TRI)</button>
                        <button id="btnCopiarMaximas" class="btn-sm secondary">Copiar Notas Máximas (TRI)</button>
                    </div>
                    <div id="mediaSimplesDisplay">Média Simples: --</div>
                </div>

                <div class="form-section">
                    <h2>Escolha o Curso</h2>
                    <div id="cursosListContainer">
                        <div id="cursosList">
                            </div>
                    </div>
                </div>

                <div class="button-group">
                    <button id="btnVerificarAprovacao" class="success">Verificar Aprovação</button>
                </div>
            </div>

            <div class="right-column">
                <h2>Resultados Ao Vivo (Simulado UFS - AC)</h2>
                <div class="live-results-table-container">
                    <table id="liveResultsTable">
                        <thead>
                            <tr>
                                <th data-sort="nome">Curso</th>
                                <th data-sort="status">Status</th>
                                <th data-sort="suaNota">Sua Nota</th>
                                <th data-sort="notaCorte">Nota Corte</th>
                            </tr>
                        </thead>
                        <tbody>
                            </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <div id="resultadoModal" class="modal">
        <div class="modal-content" id="resultadoModalContent">
            <div class="modal-header">
                <h2 id="resultadoModalTitle">Resultado</h2>
                <button class="close-button" id="closeResultadoModal">&times;</button>
            </div>
            <div class="modal-body" id="resultadoModalBody">
                </div>
        </div>
    </div>
    
    <div id="graficosModal" class="modal">
        <div class="modal-content graficos-modal-content" id="graficosModalContent">
             <div class="modal-header">
                <h2 id="graficosModalTitle">Gráficos de Desempenho TRI</h2>
                <button class="close-button" id="closeGraficosModal">&times;</button>
            </div>
            <div class="modal-body" id="graficosModalBody">
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
                    <div><canvas id="graficoHumanas"></canvas></div>
                    <div><canvas id="graficoNatureza"></canvas></div>
                    <div><canvas id="graficoLinguagens"></canvas></div>
                    <div><canvas id="graficoMatematica"></canvas></div>
                </div>
                 <div id="graficoLegenda" style="margin-top: 20px; text-align: center; font-size: 0.9em;">
                    </div>
            </div>
        </div>
    </div>


    <script>
        // --- BIBLIOTECAS DE DADOS (Como solicitado) ---
        // Exemplo com 2-3 entradas. Você precisará completar com seus dados.

        const cursosData = [
            // Formato: { id: Num, nome: "Nome Curso", pesoL: Num, pesoM: Num, pesoH: Num, pesoN: Num, pesoR: Num, notaCorte: Num }
            // Lembre-se que os pesos no CSV original são: L, M, H, N, R
            { "id": 1, "nome": "Administração", "pesoL": 2, "pesoM": 2.5, "pesoH": 2, "pesoN": 1, "pesoR": 2.5, "notaCorte": 677.62 },
  { "id": 2, "nome": "Artes Visuais", "pesoL": 2, "pesoM": 1, "pesoH": 3, "pesoN": 1, "pesoR": 3, "notaCorte": 641.09 },
  { "id": 3, "nome": "Astronomia", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 2, "pesoR": 2, "notaCorte": 685.13 },
  { "id": 4, "nome": "Biblioteconomia E Documentação", "pesoL": 2.5, "pesoM": 1.5, "pesoH": 2, "pesoN": 1, "pesoR": 3, "notaCorte": 600.69 },
  { "id": 5, "nome": "Cinema E Audiovisual", "pesoL": 2, "pesoM": 1, "pesoH": 3, "pesoN": 1, "pesoR": 3, "notaCorte": 672.7 },
  { "id": 6, "nome": "Ciência Da Computação", "pesoL": 1.5, "pesoM": 3.5, "pesoH": 1, "pesoN": 1.5, "pesoR": 2.5, "notaCorte": 759.55 },
  { "id": 7, "nome": "Ciências Atuariais", "pesoL": 1.5, "pesoM": 3.5, "pesoH": 1.5, "pesoN": 1, "pesoR": 2.5, "notaCorte": 590.91 },
  { "id": 8, "nome": "Ciências Biológicas - Bacharelado", "pesoL": 2, "pesoM": 1, "pesoH": 1, "pesoN": 3, "pesoR": 3, "notaCorte": 708.73 },
  { "id": 9, "nome": "Ciências Biológicas - Licenciatura", "pesoL": 2, "pesoM": 1, "pesoH": 1, "pesoN": 3, "pesoR": 3, "notaCorte": 690.87 },
  { "id": 10, "nome": "Ciências Contábeis", "pesoL": 2.5, "pesoM": 2.5, "pesoH": 1.5, "pesoN": 1, "pesoR": 2.5, "notaCorte": 661.29 },
  { "id": 11, "nome": "Ciências Da Religião", "pesoL": 3, "pesoM": 1, "pesoH": 3, "pesoN": 1, "pesoR": 2, "notaCorte": 566.53 },
  { "id": 12, "nome": "Ciências Econômicas", "pesoL": 2, "pesoM": 3, "pesoH": 2, "pesoN": 1, "pesoR": 2, "notaCorte": 647.27 },
  { "id": 13, "nome": "Ciências Sociais", "pesoL": 3, "pesoM": 1, "pesoH": 3, "pesoN": 1, "pesoR": 2, "notaCorte": 643.08 },
  { "id": 14, "nome": "Dança", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1.5, "pesoR": 2.5, "notaCorte": 591.78 },
  { "id": 15, "nome": "Design", "pesoL": 2, "pesoM": 1.5, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 682.37 },
  { "id": 16, "nome": "Direito", "pesoL": 2.5, "pesoM": 1.5, "pesoH": 2, "pesoN": 1, "pesoR": 3, "notaCorte": 774.11 },
  { "id": 17, "nome": "Ecologia", "pesoL": 1.5, "pesoM": 1.5, "pesoH": 1, "pesoN": 3.5, "pesoR": 2.5, "notaCorte": 592.35 },
  { "id": 18, "nome": "Educação Física - Bacharelado", "pesoL": 2, "pesoM": 1.5, "pesoH": 1.5, "pesoN": 3, "pesoR": 2, "notaCorte": 656.21 },
  { "id": 19, "nome": "Educação Física - Licenciatura", "pesoL": 2, "pesoM": 1.5, "pesoH": 1.5, "pesoN": 3, "pesoR": 2, "notaCorte": 642.44 },
  { "id": 20, "nome": "Enfermagem", "pesoL": 2, "pesoM": 1.5, "pesoH": 1.5, "pesoN": 3, "pesoR": 2, "notaCorte": 720.67 },
  { "id": 21, "nome": "Engenharia Agronômica", "pesoL": 1, "pesoM": 3, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 657.82 },
  { "id": 22, "nome": "Engenharia Agrícola", "pesoL": 1, "pesoM": 3, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 589.89 },
  { "id": 23, "nome": "Engenharia Ambiental E Sanitária", "pesoL": 1.5, "pesoM": 3, "pesoH": 1, "pesoN": 2, "pesoR": 2.5, "notaCorte": 647.52 },
  { "id": 24, "nome": "Engenharia Civil", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 2, "pesoR": 2, "notaCorte": 672.4 },
  { "id": 25, "nome": "Engenharia De Alimentos", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 2, "pesoR": 2, "notaCorte": 594.36 },
  { "id": 26, "nome": "Engenharia De Computação", "pesoL": 1.5, "pesoM": 3.5, "pesoH": 1, "pesoN": 1.5, "pesoR": 2.5, "notaCorte": 764.79 },
  { "id": 27, "nome": "Engenharia De Materiais", "pesoL": 1, "pesoM": 3, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 577.17 },
  { "id": 28, "nome": "Engenharia De Pesca", "pesoL": 1.5, "pesoM": 2.5, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 555.83 },
  { "id": 29, "nome": "Engenharia De Petróleo", "pesoL": 1.5, "pesoM": 3, "pesoH": 1, "pesoN": 2, "pesoR": 2.5, "notaCorte": 652.63 },
  { "id": 30, "nome": "Engenharia De Produção", "pesoL": 2, "pesoM": 2, "pesoH": 1.5, "pesoN": 2, "pesoR": 2.5, "notaCorte": 666.44 },
  { "id": 31, "nome": "Engenharia Eletrônica", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 1, "pesoR": 3, "notaCorte": 684.72 },
  { "id": 32, "nome": "Engenharia Elétrica", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 1, "pesoR": 3, "notaCorte": 718.3 },
  { "id": 33, "nome": "Engenharia Florestal", "pesoL": 1, "pesoM": 3, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 563.6 },
  { "id": 34, "nome": "Engenharia Mecânica", "pesoL": 1, "pesoM": 3, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 695.96 },
  { "id": 35, "nome": "Engenharia Química", "pesoL": 1.5, "pesoM": 3, "pesoH": 1, "pesoN": 2.5, "pesoR": 2, "notaCorte": 684.89 },
  { "id": 36, "nome": "Estatística", "pesoL": 2, "pesoM": 4, "pesoH": 1, "pesoN": 1, "pesoR": 2, "notaCorte": 598.65 },
  { "id": 37, "nome": "Farmácia", "pesoL": 1.5, "pesoM": 2, "pesoH": 1, "pesoN": 3.5, "pesoR": 2, "notaCorte": 687.35 },
  { "id": 38, "nome": "Filosofia", "pesoL": 2, "pesoM": 1.5, "pesoH": 1.5, "pesoN": 1.5, "pesoR": 3.5, "notaCorte": 670.83 },
  { "id": 39, "nome": "Fisioterapia", "pesoL": 2, "pesoM": 2, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 706.36 },
  { "id": 40, "nome": "Fonoaudiologia", "pesoL": 2, "pesoM": 1.5, "pesoH": 1.5, "pesoN": 3, "pesoR": 2, "notaCorte": 686.89 },
  { "id": 41, "nome": "Física - Bacharelado", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 2, "pesoR": 2, "notaCorte": 542.84 },
  { "id": 42, "nome": "Física - Licenciatura", "pesoL": 1.5, "pesoM": 2.5, "pesoH": 1, "pesoN": 2.5, "pesoR": 2.5, "notaCorte": 581.84 },
  { "id": 43, "nome": "Física Médica", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 2, "pesoR": 2, "notaCorte": 696.96 },
  { "id": 44, "nome": "Geografia - Bacharelado", "pesoL": 2, "pesoM": 1.5, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 636.35 },
  { "id": 45, "nome": "Geografia - Licenciatura", "pesoL": 2.5, "pesoM": 1, "pesoH": 3, "pesoN": 1, "pesoR": 2.5, "notaCorte": 636.92 },
  { "id": 46, "nome": "Geologia", "pesoL": 1.5, "pesoM": 3, "pesoH": 1, "pesoN": 2.5, "pesoR": 2, "notaCorte": 603.21 },
  { "id": 47, "nome": "História", "pesoL": 2, "pesoM": 1.5, "pesoH": 2.5, "pesoN": 1.5, "pesoR": 2.5, "notaCorte": 675.38 },
  { "id": 48, "nome": "Jornalismo", "pesoL": 2, "pesoM": 2, "pesoH": 2, "pesoN": 1, "pesoR": 3, "notaCorte": 703.85 },
  { "id": 49, "nome": "Letras - Espanhol", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 625.82 },
  { "id": 50, "nome": "Letras - Inglês", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 665.09 },
  { "id": 51, "nome": "Letras - Língua Portuguesa", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 665.82 },
  { "id": 52, "nome": "Letras - Português E Espanhol", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 645.32 },
  { "id": 53, "nome": "Letras - Português E Francês", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 623.12 },
  { "id": 54, "nome": "Letras - Português E Inglês", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 685.34 },
  { "id": 55, "nome": "Matemática - Bacharelado", "pesoL": 1.5, "pesoM": 3.5, "pesoH": 1, "pesoN": 2, "pesoR": 2, "notaCorte": 597.24 },
  { "id": 56, "nome": "Matemática - Licenciatura", "pesoL": 2, "pesoM": 3, "pesoH": 1, "pesoN": 1.5, "pesoR": 2.5, "notaCorte": 672.03 }, { "id": 57, "nome": "Matemática Aplicada E Computacional", "pesoL": 1.5, "pesoM": 3.5, "pesoH": 1, "pesoN": 2, "pesoR": 2, "notaCorte": 642.53 },
  { "id": 58, "nome": "Medicina", "pesoL": 2, "pesoM": 1.5, "pesoH": 1, "pesoN": 3.5, "pesoR": 2, "notaCorte": 789.39 },
  { "id": 59, "nome": "Medicina Veterinária", "pesoL": 1.5, "pesoM": 1.5, "pesoH": 1.5, "pesoN": 3.5, "pesoR": 2, "notaCorte": 697.05 },
  { "id": 60, "nome": "Nutrição", "pesoL": 2, "pesoM": 2, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 686.51 },
  { "id": 61, "nome": "Odontologia", "pesoL": 2, "pesoM": 2, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 751.53 },
  { "id": 62, "nome": "Pedagogia", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 669.18 },
  { "id": 63, "nome": "Psicologia", "pesoL": 2, "pesoM": 1, "pesoH": 2.5, "pesoN": 1.5, "pesoR": 3, "notaCorte": 755.87 },
  { "id": 64, "nome": "Publicidade E Propaganda", "pesoL": 2, "pesoM": 1.5, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 708.59 },
  { "id": 65, "nome": "Química - Bacharelado", "pesoL": 1.5, "pesoM": 3, "pesoH": 1, "pesoN": 2.5, "pesoR": 2, "notaCorte": 616.0 },
  { "id": 66, "nome": "Química - Licenciatura", "pesoL": 2, "pesoM": 2, "pesoH": 1, "pesoN": 2.5, "pesoR": 2.5, "notaCorte": 617.12 },
  { "id": 67, "nome": "Química Industrial", "pesoL": 1.5, "pesoM": 2, "pesoH": 1, "pesoN": 2.5, "pesoR": 3, "notaCorte": 624.18 },
  { "id": 68, "nome": "Relações Internacionais", "pesoL": 3, "pesoM": 1.5, "pesoH": 2.5, "pesoN": 1, "pesoR": 2, "notaCorte": 692.88 },
  { "id": 69, "nome": "Secretariado Executivo", "pesoL": 2.5, "pesoM": 1.5, "pesoH": 2, "pesoN": 1, "pesoR": 3, "notaCorte": 608.91 },
  { "id": 70, "nome": "Serviço Social", "pesoL": 2.5, "pesoM": 1, "pesoH": 2.5, "pesoN": 1, "pesoR": 3, "notaCorte": 648.94 },
  { "id": 71, "nome": "Sistemas De Informação", "pesoL": 2, "pesoM": 3, "pesoH": 1.5, "pesoN": 1, "pesoR": 2.5, "notaCorte": 734.62 },
  { "id": 72, "nome": "Teatro", "pesoL": 2, "pesoM": 1, "pesoH": 3, "pesoN": 1, "pesoR": 3, "notaCorte": 605.03 },
  { "id": 73, "nome": "Turismo", "pesoL": 2.5, "pesoM": 2, "pesoH": 2.5, "pesoN": 1, "pesoR": 2, "notaCorte": 592.09 },
  { "id": 74, "nome": "Zootecnia", "pesoL": 1.5, "pesoM": 2.5, "pesoH": 1, "pesoN": 3, "pesoR": 2, "notaCorte": 612.45 }
            
        ];

        const notasTRIData = {
            // Formato: area: { acertos: [notaMin, notaMed, notaMax] }
            // Áreas: 'ling', 'hum', 'nat', 'mat'
            'ling': {
                "1": [298.8, 307.2, 353.4],
                "2": [298.8, 316.9, 378.1],
                "3": [299.0, 325.9, 393.4],
                "4": [299.0, 337.2, 406.8],
                "5": [299.4, 346.7, 418.1],
                "6": [300.2, 357.5, 429.3],
                "7": [300.8, 368.9, 445.5],
                "8": [302.3, 381.1, 455.7],
                "9": [303.2, 393.9, 466.7],
                "10": [304.3, 407.0, 480.3],
                "11": [306.4, 420.9, 489.6],
                "12": [309.9, 434.7, 493.8],
                "13": [312.8, 448.2, 507.1],
                "14": [306.7, 460.6, 515.5],
                "15": [323.7, 472.1, 525.7],
                "16": [331.1, 482.7, 529.1],
                "17": [327.1, 492.3, 537.2],
                "18": [362.5, 501.1, 544.0],
                "19": [366.0, 509.5, 548.3],
                "20": [360.8, 517.3, 553.5],
                "21": [410.1, 524.7, 558.9],
                "22": [420.6, 531.9, 566.3],
                "23": [461.7, 538.8, 571.8],
                "24": [480.0, 545.7, 580.4],
                "25": [493.3, 552.5, 586.4],
                "26": [502.0, 559.1, 593.0],
                "27": [507.2, 565.9, 599.2],
                "28": [519.7, 572.7, 607.6],
                "29": [531.7, 579.6, 616.8],
                "30": [537.3, 586.6, 622.9],
                "31": [541.7, 593.7, 633.6],
                "32": [551.7, 601.0, 640.9],
                "33": [564.6, 608.6, 646.7],
                "34": [571.6, 616.7, 652.9],
                "35": [581.4, 624.9, 664.3],
                "36": [586.7, 633.8, 672.1],
                "37": [595.4, 643.3, 683.9],
                "38": [609.8, 653.5, 694.4],
                "39": [614.7, 665.0, 706.5],
                "40": [630.8, 677.8, 724.2],
                "41": [647.6, 692.0, 739.9],
                "42": [665.1, 709.4, 757.8],
                "43": [674.0, 729.9, 765.7],
                "44": [707.0, 755.2, 777.4],
                "45": [795.8, 795.8, 795.8]
            },
            'hum': {
                "1": [283.8, 292.7, 327.5],
                "2": [283.9, 302.7, 358.6],
                "3": [284.2, 312.8, 378.5],
                "4": [284.2, 323.7, 395.3],
                "5": [284.7, 334.4, 411.3],
                "6": [285.4, 346.8, 424.3],
                "7": [286.2, 359.0, 436.7],
                "8": [285.6, 372.5, 449.5],
                "9": [286.7, 386.3, 462.1],
                "10": [291.3, 401.3, 473.5],
                "11": [291.1, 416.5, 483.8],
                "12": [294.0, 432.1, 497.8],
                "13": [300.6, 447.8, 507.6],
                "14": [301.8, 462.8, 518.7],
                "15": [309.6, 477.2, 529.0],
                "16": [339.8, 490.8, 540.6],
                "17": [362.9, 503.2, 552.2],
                "18": [366.8, 514.5, 557.9],
                "19": [387.3, 524.9, 564.9],
                "20": [414.0, 534.4, 573.2],
                "21": [466.4, 543.4, 580.2],
                "22": [476.3, 551.9, 588.9],
                "23": [488.5, 560.1, 595.8],
                "24": [511.9, 568.1, 602.7],
                "25": [526.1, 575.8, 610.7],
                "26": [538.0, 583.5, 617.8],
                "27": [540.3, 591.3, 622.5],
                "28": [548.7, 599.1, 629.6],
                "29": [562.8, 607.0, 637.9],
                "30": [571.9, 615.2, 645.9],
                "31": [571.7, 623.5, 655.5],
                "32": [589.8, 632.1, 664.5],
                "33": [594.5, 641.1, 675.3],
                "34": [600.7, 650.5, 689.4],
                "35": [619.2, 660.3, 699.5],
                "36": [621.2, 670.7, 709.5],
                "37": [636.9, 681.9, 719.2],
                "38": [650.4, 693.7, 733.8],
                "39": [652.4, 706.7, 748.2],
                "40": [672.9, 720.9, 762.9],
                "41": [692.4, 736.3, 778.0],
                "42": [705.9, 754.5, 795.0],
                "43": [727.7, 774.5, 808.2],
                "44": [762.7, 797.2, 817.4],
                "45": [819.7, 819.7, 819.7]
            },
            'nat': {
                "1": [342.1, 348.7, 400.1],
                "2": [342.1, 356.0, 430.3],
                "3": [342.2, 363.5, 446.9],
                "4": [342.3, 371.4, 463.7],
                "5": [342.4, 380.6, 479.5],
                "6": [343.0, 390.7, 491.2],
                "7": [343.1, 401.8, 506.5],
                "8": [344.6, 414.2, 518.5],
                "9": [346.2, 427.7, 538.1],
                "10": [345.6, 442.6, 549.7],
                "11": [352.8, 458.9, 558.0],
                "12": [354.2, 475.9, 576.5],
                "13": [355.5, 493.4, 581.6],
                "14": [365.5, 510.7, 589.4],
                "15": [372.4, 527.2, 597.7],
                "16": [386.2, 542.4, 608.8],
                "17": [413.8, 556.0, 614.5],
                "18": [423.8, 568.1, 620.8],
                "19": [434.8, 578.9, 629.8],
                "20": [492.5, 588.9, 635.5],
                "21": [513.5, 598.3, 641.5],
                "22": [538.0, 607.2, 651.4],
                "23": [547.7, 615.8, 657.7],
                "24": [563.9, 624.0, 661.2],
                "25": [577.2, 632.4, 670.8],
                "26": [585.9, 640.6, 677.3],
                "27": [598.9, 648.6, 688.7],
                "28": [607.9, 657.0, 693.9],
                "29": [621.6, 665.3, 706.3],
                "30": [629.4, 673.6, 707.0],
                "31": [637.5, 682.1, 718.7],
                "32": [644.6, 691.0, 728.9],
                "33": [656.8, 700.2, 736.9],
                "34": [664.3, 709.0, 745.4],
                "35": [677.6, 719.0, 755.1],
                "36": [688.2, 729.1, 767.5],
                "37": [695.4, 740.2, 773.3],
                "38": [717.1, 751.8, 788.0],
                "39": [721.5, 764.0, 799.2],
                "40": [727.2, 777.8, 817.2],
                "41": [764.4, 795.6, 822.8],
                "42": [781.8, 817.4, 844.5],
                "43": [812.6, 839.9, 854.0],
                "44": [867.2, 867.2, 867.2],
                "45": [867.2, 867.2, 867.2]
            },
            'mat': {
                "1": [371.0, 376.9, 409.3],
                "2": [371.0, 382.4, 445.0],
                "3": [371.1, 389.3, 479.0],
                "4": [371.3, 396.5, 509.1],
                "5": [371.4, 404.8, 535.9],
                "6": [371.8, 414.3, 557.4],
                "7": [372.4, 425.1, 576.1],
                "8": [373.0, 437.5, 597.5],
                "9": [373.8, 451.9, 609.3],
                "10": [375.2, 468.9, 623.1],
                "11": [376.7, 488.2, 633.3],
                "12": [379.1, 510.4, 643.6],
                "13": [381.3, 534.5, 653.1],
                "14": [386.0, 559.8, 662.3],
                "15": [391.3, 584.0, 670.3],
                "16": [397.9, 606.3, 678.1],
                "17": [397.7, 625.3, 687.4],
                "18": [419.3, 641.4, 694.8],
                "19": [436.4, 655.3, 701.9],
                "20": [460.5, 667.2, 708.9],
                "21": [499.7, 678.3, 719.6],
                "22": [541.1, 688.4, 725.4],
                "23": [583.4, 698.3, 735.9],
                "24": [598.1, 707.9, 743.2],
                "25": [612.6, 717.2, 752.4],
                "26": [632.4, 726.5, 757.8],
                "27": [663.9, 735.5, 771.2],
                "28": [673.8, 744.5, 778.5],
                "29": [691.4, 753.5, 788.1],
                "30": [702.1, 762.8, 795.9],
                "31": [722.7, 771.6, 804.7],
                "32": [737.5, 780.8, 814.4],
                "33": [752.9, 790.5, 827.4],
                "34": [756.6, 800.0, 836.5],
                "35": [772.3, 810.1, 849.4],
                "36": [781.1, 820.0, 858.3],
                "37": [787.6, 831.4, 867.5],
                "38": [805.0, 842.7, 877.0],
                "39": [814.6, 855.1, 890.5],
                "40": [830.7, 868.4, 901.2],
                "41": [841.6, 882.4, 915.1],
                "42": [857.2, 899.1, 927.7],
                "43": [876.8, 918.2, 940.6],
                "44": [904.9, 938.6, 951.8],
                "45": [961.9, 961.9, 961.9]
            }
            // Adicione os dados completos para todas as áreas e acertos aqui...
        };

        // --- LÓGICA DO APLICATIVO ---
        document.addEventListener('DOMContentLoaded', () => {
            // Elementos da UI
            const acertosEnemContainer = document.getElementById('acertosEnemContainer');
            const totalAcertosDisplay = document.getElementById('totalAcertosDisplay');
            const btnGraficos = document.getElementById('btnGraficos');
            
            const notasEnemContainer = document.getElementById('notasEnemContainer');
            const btnCopiarMinimas = document.getElementById('btnCopiarMinimas');
            const btnCopiarMedias = document.getElementById('btnCopiarMedias');
            const btnCopiarMaximas = document.getElementById('btnCopiarMaximas');
            const mediaSimplesDisplay = document.getElementById('mediaSimplesDisplay');
            
            const cursosListDiv = document.getElementById('cursosList');
            const btnVerificarAprovacao = document.getElementById('btnVerificarAprovacao');
            
            const liveResultsTableBody = document.querySelector('#liveResultsTable tbody');
            
            const resultadoModal = document.getElementById('resultadoModal');
            const resultadoModalTitle = document.getElementById('resultadoModalTitle');
            const resultadoModalBody = document.getElementById('resultadoModalBody');
            const closeResultadoModal = document.getElementById('closeResultadoModal');

            const graficosModal = document.getElementById('graficosModal');
            const closeGraficosModal = document.getElementById('closeGraficosModal');
            let charts = {}; // Para armazenar instâncias dos gráficos

            // Ordem das áreas para inputs de acertos e display TRI (L, H, N, M)
            const areasAcertosInfo = [
                { id: 'ling', nome: 'Linguagens', inputVar: null, displayVar: null },
                { id: 'hum', nome: 'Humanas', inputVar: null, displayVar: null },
                { id: 'nat', nome: 'Naturezas', inputVar: null, displayVar: null },
                { id: 'mat', nome: 'Matemática', inputVar: null, displayVar: null }
            ];

            // Ordem das áreas para inputs de notas finais (L, M, H, N, R)
            // Esta ordem é crucial para a função calcularNotaPonderada
            const areasNotasFinaisInfo = [
                { id: 'notaL', nome: 'Linguagens', inputVar: null },
                { id: 'notaH', nome: 'Humanas', inputVar: null },
                { id: 'notaN', nome: 'Naturezas', inputVar: null },
                { id: 'notaM', nome: 'Matemática', inputVar: null },
                { id: 'notaR', nome: 'Redação', inputVar: null }
            ];

            let cursoSelecionadoId = null;

            // --- Funções Auxiliares ---
            function safeFloat(value, defaultValue = null) {
                if (typeof value !== 'string' && typeof value !== 'number') return defaultValue;
                const cleanedValue = String(value).trim().replace(',', '.');
                if (cleanedValue === '') return defaultValue;
                const num = parseFloat(cleanedValue);
                return isNaN(num) ? defaultValue : num;
            }

            function obterNotasTRI(area, acertos) {
                acertos = parseInt(acertos);
                if (isNaN(acertos) || acertos < 0) return [0, 0, 0];
                if (acertos === 0) return [0,0,0]; // TRI não definido para 0 acertos
                if (acertos > 45) acertos = 45;
                
                return notasTRIData[area]?.[acertos] || [0, 0, 0];
            }
            
            function showModal(modalElement, title, bodyHtml, isError = false) {
                const titleElement = modalElement.querySelector('.modal-header h2');
                const bodyElement = modalElement.querySelector('.modal-body');
                
                titleElement.textContent = title;
                bodyElement.innerHTML = bodyHtml;
                titleElement.style.color = isError ? 'var(--danger-color)' : 'var(--primary-color)';
                modalElement.style.display = 'flex';
            }

            // --- Inicialização da UI ---
            function criarInputsAcertos() {
                areasAcertosInfo.forEach(areaInfo => {
                    const group = document.createElement('div');
                    group.className = 'input-group';
                    
                    const label = document.createElement('label');
                    label.htmlFor = `acertos-${areaInfo.id}`;
                    label.textContent = `${areaInfo.nome}:`;
                    
                    const input = document.createElement('input');
                    input.type = 'number';
                    input.id = `acertos-${areaInfo.id}`;
                    input.className = 'acertos-input';
                    input.min = 0;
                    input.max = 45;
                    input.placeholder = "0";
                    input.autocomplete = 'off';
                    areaInfo.inputVar = input;
                    
                    const display = document.createElement('span');
                    display.id = `tri-${areaInfo.id}`;
                    display.className = 'tri-display';
                    display.textContent = 'Min: 0.0 | Média: 0.0 | Max: 0.0';
                    areaInfo.displayVar = display;
                    
                    group.appendChild(label);
                    group.appendChild(input);
                    group.appendChild(display);
                    acertosEnemContainer.appendChild(group);

                    input.addEventListener('input', () => {
                        let val = parseInt(input.value);
                        if (isNaN(val) || val < 0) input.value = ''; // Limpa se inválido ou negativo
                        else if (val > 45) input.value = 45; // Limita a 45
                        atualizarDisplayTRI(areaInfo.id, input.value);
                        atualizarTotalAcertos();
                    });
                });
            }

            function criarInputsNotasFinais() {
                areasNotasFinaisInfo.forEach(areaInfo => {
                    const group = document.createElement('div');
                    group.className = 'notas-finales-input-group';
                    
                    const label = document.createElement('label');
                    label.htmlFor = areaInfo.id;
                    label.textContent = `${areaInfo.nome}:`;
                    
                    const input = document.createElement('input');
                    input.type = 'text'; // Permite vírgula e ponto
                    input.id = areaInfo.id;
                    input.placeholder = "0.0";
                    input.autocomplete = 'off';
                    areaInfo.inputVar = input;
                    
                    group.appendChild(label);
                    group.appendChild(input);
                    notasEnemContainer.appendChild(group);

                    input.addEventListener('input', () => {
                        validarInputNota(input);
                        calcularMediaSimplesAoVivo();
                        atualizarResultadosAoVivo();
                    });
                });
            }
            
            function validarInputNota(inputElement) {
                let value = inputElement.value.trim();
                value = value.replace(',', '.');
                
                if (value === '' || value === '-') { // Permite campo vazio ou apenas '-'
                    return;
                }

                // Validação mais simples: tenta converter para float e checar range
                const num = parseFloat(value);
                if (isNaN(num)) {
                    inputElement.value = value.slice(0, -1); // Remove último char inválido
                } else if (num < 0) {
                    inputElement.value = '0';
                } else if (num > 1000) {
                    inputElement.value = '1000';
                }
            }

            function popularListaCursos() {
                cursosListDiv.innerHTML = ''; // Limpa lista
                cursosData.sort((a, b) => a.nome.localeCompare(b.nome)).forEach(curso => {
                    const item = document.createElement('div');
                    item.className = 'curso-item';
                    item.textContent = `${curso.id}: ${curso.nome}`;
                    item.dataset.cursoId = curso.id;
                    item.addEventListener('click', () => {
                        if (cursoSelecionadoId === curso.id) {
                            cursoSelecionadoId = null;
                            item.classList.remove('selected');
                        } else {
                            if (cursoSelecionadoId) {
                                const prevSelected = cursosListDiv.querySelector(`.curso-item[data-curso-id="${cursoSelecionadoId}"]`);
                                if (prevSelected) prevSelected.classList.remove('selected');
                            }
                            cursoSelecionadoId = curso.id;
                            item.classList.add('selected');
                        }
                    });
                    cursosListDiv.appendChild(item);
                });
                if (cursosData.length === 0) {
                    cursosListDiv.textContent = "Nenhum curso carregado.";
                    btnVerificarAprovacao.disabled = true;
                } else {
                     btnVerificarAprovacao.disabled = false;
                }
            }

            // --- Lógica de Cálculo e Atualização ---
            function atualizarDisplayTRI(areaId, acertosStr) {
                const areaInfo = areasAcertosInfo.find(a => a.id === areaId);
                if (!areaInfo) return;

                const acertos = parseInt(acertosStr) || 0;
                const [min, med, max] = obterNotasTRI(areaId, acertos);
                areaInfo.displayVar.textContent = `Min: ${min.toFixed(1)} | Média: ${med.toFixed(1)} | Max: ${max.toFixed(1)}`;
            }

            function atualizarTotalAcertos() {
                let total = 0;
                areasAcertosInfo.forEach(areaInfo => {
                    total += parseInt(areaInfo.inputVar.value) || 0;
                });
                totalAcertosDisplay.textContent = `Total: ${String(total).padStart(2, '0')}/180`;
            }

            function copiarNotas(tipoNotaIndex) { // 0=min, 1=med, 2=max
                const notasParaCopiar = {}; // L, M, H, N
                
                areasAcertosInfo.forEach(areaInfo => { // L, H, N, M
                    const acertos = parseInt(areaInfo.inputVar.value) || 0;
                    const notas = obterNotasTRI(areaInfo.id, acertos);
                    // Mapeia do nome da área (ling, hum, nat, mat) para as chaves de nota (L, M, H, N)
                    if (areaInfo.id === 'ling') notasParaCopiar['L'] = notas[tipoNotaIndex];
                    else if (areaInfo.id === 'hum') notasParaCopiar['H'] = notas[tipoNotaIndex];
                    else if (areaInfo.id === 'nat') notasParaCopiar['N'] = notas[tipoNotaIndex];
                    else if (areaInfo.id === 'mat') notasParaCopiar['M'] = notas[tipoNotaIndex];
                });

                areasNotasFinaisInfo.forEach(notaInfo => { // L, M, H, N, R
                    let notaParaSetar = 0;
                    if (notaInfo.id === 'notaL') notaParaSetar = notasParaCopiar['L'];
                    else if (notaInfo.id === 'notaM') notaParaSetar = notasParaCopiar['M'];
                    else if (notaInfo.id === 'notaH') notaParaSetar = notasParaCopiar['H'];
                    else if (notaInfo.id === 'notaN') notaParaSetar = notasParaCopiar['N'];
                    // Redação (notaR) não é copiada do TRI
                    
                    if (notaInfo.id !== 'notaR' && notaParaSetar !== undefined) {
                        notaInfo.inputVar.value = notaParaSetar.toFixed(1);
                    }
                });
                calcularMediaSimplesAoVivo();
                atualizarResultadosAoVivo();
            }
            
            function getNotasFinaisArray() { // Retorna [L, M, H, N, R]
                const notas = [];
                areasNotasFinaisInfo.forEach(areaInfo => {
                    const val = safeFloat(areaInfo.inputVar.value, 0.0);
                    notas.push(val > 1000 ? 1000 : val < 0 ? 0 : val); // Garante 0-1000
                });
                return notas;
            }

            function calcularMediaSimplesAoVivo() {
                const notas = getNotasFinaisArray();
                const notasValidas = notas.filter(n => n !== null && n > 0); // Considera 0 como não informado para média
                
                let hasInvalidText = false;
                areasNotasFinaisInfo.forEach(areaInfo => {
                    if (areaInfo.inputVar.value.trim() !== '' && safeFloat(areaInfo.inputVar.value) === null) {
                        hasInvalidText = true;
                    }
                });

                if (hasInvalidText) {
                    mediaSimplesDisplay.textContent = "Média Simples: Erro";
                    return;
                }

                if (notasValidas.length > 0) {
                    const media = notasValidas.reduce((sum, n) => sum + n, 0) / notasValidas.length;
                    mediaSimplesDisplay.textContent = `Média Simples: ${media.toFixed(2)}`;
                } else {
                     if (notas.every(n => n === 0 || n === null)) { // se todos os campos estão vazios ou 0
                        mediaSimplesDisplay.textContent = "Média Simples: --";
                     } else {
                        mediaSimplesDisplay.textContent = "Média Simples: 0.00"; // se alguns são 0 e outros vazios
                     }
                }
            }

            function calcularNotaPonderada(pesos, notasEnem) { // pesos: {L,M,H,N,R}, notasEnem: [L,M,H,N,R]
                const somaPesos = pesos.pesoL + pesos.pesoM + pesos.pesoH + pesos.pesoN + pesos.pesoR;
                if (somaPesos === 0) return 0.0;

                const mediaPonderada = (
                    (pesos.pesoL * notasEnem[0]) + // Linguagens
                    (pesos.pesoH * notasEnem[1]) + // Humanas
                    (pesos.pesoN * notasEnem[2]) + // Natureza
                    (pesos.pesoM * notasEnem[3]) + // Matemática
                    (pesos.pesoR * notasEnem[4])   // Redação
                ) / somaPesos;
                return mediaPonderada;
            }

            let currentSort = { column: 'nome', reverse: false };
            function atualizarResultadosAoVivo() {
                liveResultsTableBody.innerHTML = ''; // Limpa tabela
                const notasEnem = getNotasFinaisArray(); // [L, M, H, N, R]

                let hasInvalidText = false;
                let hasOutOfRange = false;
                areasNotasFinaisInfo.forEach((areaInfo, index) => {
                     const rawValue = areaInfo.inputVar.value.trim();
                     const floatValue = safeFloat(rawValue);
                     if (rawValue !== '' && floatValue === null) {
                        hasInvalidText = true;
                     } else if (floatValue !== null && (floatValue < 0 || floatValue > 1000)) {
                        hasOutOfRange = true;
                     }
                });
                
                const dataForTable = [];

                cursosData.forEach(curso => {
                    const pesos = { pesoL: curso.pesoL, pesoM: curso.pesoM, pesoH: curso.pesoH, pesoN: curso.pesoN, pesoR: curso.pesoR };
                    const mediaCalculada = calcularNotaPonderada(pesos, notasEnem);
                    let status = "";
                    let statusClass = "";

                    if (hasInvalidText) {
                        status = "Entrada Inválida";
                        statusClass = 'status-invalido';
                    } else if (hasOutOfRange) {
                         status = "Nota Fora (0-1000)";
                         statusClass = 'status-invalido';
                    } else {
                        if (mediaCalculada >= curso.notaCorte) {
                            status = "APROVADO";
                            statusClass = 'status-aprovado';
                        } else {
                            status = "EXCEDENTE";
                            statusClass = 'status-excedente';
                        }
                    }
                    dataForTable.push({
                        nome: curso.nome,
                        status: status,
                        statusClass: statusClass,
                        suaNota: mediaCalculada,
                        notaCorte: curso.notaCorte
                    });
                });

                // Aplicar ordenação
                dataForTable.sort((a, b) => {
                    let valA = a[currentSort.column];
                    let valB = b[currentSort.column];

                    if (currentSort.column === 'suaNota' || currentSort.column === 'notaCorte') {
                        valA = parseFloat(valA);
                        valB = parseFloat(valB);
                    } else if (currentSort.column === 'nome' || currentSort.column === 'status') {
                        valA = String(valA).toLowerCase();
                        valB = String(valB).toLowerCase();
                    }
                    
                    if (valA < valB) return currentSort.reverse ? 1 : -1;
                    if (valA > valB) return currentSort.reverse ? -1 : 1;
                    return 0;
                });


                dataForTable.forEach(item => {
                    const row = liveResultsTableBody.insertRow();
                    row.insertCell().textContent = item.nome;
                    const statusCell = row.insertCell();
                    statusCell.textContent = item.status;
                    statusCell.className = item.statusClass;
                    row.insertCell().textContent = item.suaNota.toFixed(2);
                    row.insertCell().textContent = item.notaCorte.toFixed(2);
                });
            }
            
            document.querySelectorAll('#liveResultsTable th').forEach(headerCell => {
                headerCell.addEventListener('click', () => {
                    const columnToSort = headerCell.dataset.sort;
                    if (!columnToSort) return; // Não ordenar colunas sem data-sort

                    if (currentSort.column === columnToSort) {
                        currentSort.reverse = !currentSort.reverse;
                    } else {
                        currentSort.column = columnToSort;
                        currentSort.reverse = false;
                    }
                    atualizarResultadosAoVivo(); // Re-renderiza a tabela com a nova ordenação
                });
            });


            function verificarAprovacao() {
                if (!cursoSelecionadoId) {
                    showModal(resultadoModal, "Erro de Seleção", "<p>Por favor, selecione um curso na lista.</p>", true);
                    return;
                }

                const curso = cursosData.find(c => c.id === cursoSelecionadoId);
                if (!curso) {
                    showModal(resultadoModal, "Erro Interno", "<p>Curso selecionado não encontrado nos dados.</p>", true);
                    return;
                }

                const notasEnem = getNotasFinaisArray(); // [L, M, H, N, R]
                let inputError = false;
                let errorMessages = [];

                areasNotasFinaisInfo.forEach((areaInfo, index) => {
                    const rawValue = areaInfo.inputVar.value.trim();
                    const floatValue = safeFloat(rawValue);
                    if (rawValue === '' || floatValue === null) {
                        errorMessages.push(`Nota inválida para ${areaInfo.nome}. Insira um valor numérico.`);
                        inputError = true;
                    } else if (floatValue < 0 || floatValue > 1000) {
                        errorMessages.push(`Nota para ${areaInfo.nome} (${floatValue}) fora do intervalo (0-1000).`);
                        inputError = true;
                    }
                });

                if (inputError) {
                    showModal(resultadoModal, "Erro nas Notas", `<p>${errorMessages.join("</p><p>")}</p>`, true);
                    return;
                }
                
                const pesos = { pesoL: curso.pesoL, pesoM: curso.pesoM, pesoH: curso.pesoH, pesoN: curso.pesoN, pesoR: curso.pesoR };
                const mediaCalculada = calcularNotaPonderada(pesos, notasEnem);
                const status = mediaCalculada >= curso.notaCorte ? "APROVADO" : "EXCEDENTE";
                const statusColor = status === "APROVADO" ? "var(--success-color)" : "var(--danger-color)";

                let bodyHtml = `
                    <p>Curso Selecionado: <strong>${curso.nome}</strong></p>
                    <table id="resultadoVerificacaoTable">
                        <thead><tr><th>Item</th><th>Valor</th></tr></thead>
                        <tbody>
                            <tr><td>Sua Média Ponderada</td><td><strong>${mediaCalculada.toFixed(2)}</strong></td></tr>
                            <tr><td>Nota de Corte do Curso</td><td>${curso.notaCorte.toFixed(2)}</td></tr>
                            <tr><td>Status</td><td style="color: ${statusColor}; font-weight: bold;">${status}</td></tr>
                        </tbody>
                    </table>
                `;
                showModal(resultadoModal, `Resultado para ${curso.nome}`, bodyHtml);
            }
            
            function plotGrafico(chartId, label, data, color) {
                 const ctx = document.getElementById(chartId).getContext('2d');
                 if (charts[chartId]) {
                     charts[chartId].destroy(); // Destrói gráfico anterior se existir
                 }
                 charts[chartId] = new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: ["Máx. Possível\n(45 Acertos)", "Sua Média\nAtual (TRI)", "Mín. Possível\n(1 Acerto)", "Seu Máx.\nAtual (TRI)", "Seu Mín.\nAtual (TRI)"],
                        datasets: [{
                            label: label,
                            data: data,
                            backgroundColor: [ // Cores das barras individuais
                                'rgba(54, 162, 235, 0.7)',  // Azul para Máx. Possível
                                'rgba(255, 206, 86, 0.7)', // Amarelo para Sua Média
                                'rgba(255, 99, 132, 0.7)',  // Vermelho para Mín. Possível
                                'rgba(75, 192, 192, 0.7)',  // Verde para Seu Máx.
                                'rgba(153, 102, 255, 0.7)' // Roxo para Seu Mín.
                            ],
                            borderColor: [
                                'rgba(54, 162, 235, 1)',
                                'rgba(255, 206, 86, 1)',
                                'rgba(255, 99, 132, 1)',
                                'rgba(75, 192, 192, 1)',
                                'rgba(153, 102, 255, 1)'
                            ],
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: true,
                        plugins: {
                           legend: { display: false }, // Remove a legenda principal do Chart.js
                           title: { display: true, text: label, font: { size: 14 } }
                        },
                        scales: {
                            y: {
                                beginAtZero: true,
                                max: 1000, // Ou um valor dinâmico se preferir
                                ticks: { stepSize: 100 }
                            },
                            x: {
                                ticks: { font: {size: 9} }
                            }
                        }
                    }
                });
            }

            function abrirJanelaGraficos() {
                const acertosValidos = {};
                let algumAcerto = false;
                areasAcertosInfo.forEach(area => { // L, H, N, M
                    const acertos = parseInt(area.inputVar.value) || 0;
                    acertosValidos[area.id] = acertos;
                    if (acertos > 0) algumAcerto = true;
                });

                // Mapeamento de areasAcertosInfo (L,H,N,M) para os gráficos e dados TRI
                const plotConfig = [
                    { id: 'hum', chartId: 'graficoHumanas', label: 'Ciências Humanas', currentAcertos: acertosValidos['hum'] },
                    { id: 'nat', chartId: 'graficoNatureza', label: 'Ciências da Natureza', currentAcertos: acertosValidos['nat'] },
                    { id: 'ling', chartId: 'graficoLinguagens', label: 'Linguagens e Códigos', currentAcertos: acertosValidos['ling'] },
                    { id: 'mat', chartId: 'graficoMatematica', label: 'Matemática', currentAcertos: acertosValidos['mat'] }
                ];
                
                plotConfig.forEach(config => {
                    const notasMinPossivel = obterNotasTRI(config.id, 1);       // [min, med, max]
                    const notasMaxPossivel = obterNotasTRI(config.id, 45);      // [min, med, max]
                    const notasAtuais = obterNotasTRI(config.id, config.currentAcertos); // [min, med, max]

                    const yValues = [
                        notasMaxPossivel[2], // Nota Máxima com 45 acertos
                        notasAtuais[1],      // Sua Nota Média atual
                        notasMinPossivel[0], // Nota Mínima com 1 acerto
                        notasAtuais[2],      // Sua Nota Máxima atual
                        notasAtuais[0]       // Sua Nota Mínima atual
                    ];
                    plotGrafico(config.chartId, config.label, yValues);
                });
                
                document.getElementById('graficoLegenda').innerHTML = `
                    <span style="color: rgba(54, 162, 235, 1);">&#9632;</span> Máx. Possível (45)
                    <span style="color: rgba(255, 206, 86, 1); margin-left:10px;">&#9632;</span> Sua Média (TRI)
                    <span style="color: rgba(255, 99, 132, 1); margin-left:10px;">&#9632;</span> Mín. Possível (1) <br>
                    <span style="color: rgba(75, 192, 192, 1);">&#9632;</span> Seu Máx. (TRI)
                    <span style="color: rgba(153, 102, 255, 1); margin-left:10px;">&#9632;</span> Seu Mín. (TRI)
                `;


                graficosModal.style.display = 'flex';
            }


            // --- Event Listeners ---
            btnCopiarMinimas.addEventListener('click', () => copiarNotas(0));
            btnCopiarMedias.addEventListener('click', () => copiarNotas(1));
            btnCopiarMaximas.addEventListener('click', () => copiarNotas(2));
            btnVerificarAprovacao.addEventListener('click', verificarAprovacao);
            btnGraficos.addEventListener('click', abrirJanelaGraficos);

            closeResultadoModal.onclick = () => resultadoModal.style.display = "none";
            closeGraficosModal.onclick = () => graficosModal.style.display = "none";
            window.onclick = (event) => {
                if (event.target == resultadoModal) resultadoModal.style.display = "none";
                if (event.target == graficosModal) graficosModal.style.display = "none";
            };

            // --- Inicialização ---
            criarInputsAcertos();
            criarInputsNotasFinais();
            popularListaCursos();
            atualizarTotalAcertos(); // Para mostrar "Total: 00/180" inicialmente
            calcularMediaSimplesAoVivo(); // Para mostrar "Média Simples: --" inicialmente
            atualizarResultadosAoVivo(); // Para popular a tabela ao vivo inicialmente (com zeros/status de erro)
        });

    </script>
</body>
</html>
