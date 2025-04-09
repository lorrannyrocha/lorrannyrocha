<!DOCTYPE html><html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Simulador de Leitura - Árvore</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 30px;
      background-color: #eef2f5;
    }
    h1 {
      color: #2c3e50;
    }
    input, select, button {
      display: block;
      margin-top: 10px;
      padding: 10px;
      width: 100%;
      max-width: 400px;
    }
    .box {
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 8px #ccc;
      max-width: 600px;
    }
    .resultado, .historico {
      background-color: #d4edda;
      padding: 15px;
      margin-top: 20px;
      color: #155724;
      display: none;
      border-radius: 5px;
    }
    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: left;
    }
    th {
      background: #f0f0f0;
    }
  </style>
</head>
<body>  <div class="box">
    <h1>Simulador Árvore de Leitura</h1><form id="formulario">
  <label>RA do Aluno:</label>
  <input type="text" id="ra" required>

  <label>Senha:</label>
  <input type="password" id="senha" required>

  <label>Escolha o Livro:</label>
  <select id="livro">
    <option value="O Pequeno Príncipe">O Pequeno Príncipe</option>
    <option value="Dom Casmurro">Dom Casmurro</option>
    <option value="Capitães da Areia">Capitães da Areia</option>
    <option value="O Alienista">O Alienista</option>
    <option value="Memórias Póstumas de Brás Cubas">Memórias Póstumas de Brás Cubas</option>
    <option value="A Hora da Estrela">A Hora da Estrela</option>
  </select>

  <button type="submit">Marcar Livro e Responder</button>
</form>

<div class="resultado" id="resultadoBox"></div>
<div class="historico" id="historicoBox">
  <h3>Histórico de Leituras</h3>
  <table>
    <thead>
      <tr><th>RA</th><th>Livro</th><th>Data</th><th>Acertos</th></tr>
    </thead>
    <tbody id="tabelaHistorico"></tbody>
  </table>
  <button onclick="exportarCSV()">Exportar CSV</button>
</div>

  </div>  <script>
    const form = document.getElementById("formulario");
    const resultadoBox = document.getElementById("resultadoBox");
    const historicoBox = document.getElementById("historicoBox");
    const tabelaHistorico = document.getElementById("tabelaHistorico");

    function atualizarHistorico() {
      const leituras = JSON.parse(localStorage.getItem("leituras") || "[]");
      tabelaHistorico.innerHTML = "";
      leituras.forEach(item => {
        const linha = `<tr><td>${item.ra}</td><td>${item.livro}</td><td>${item.data}</td><td>${item.acertos}</td></tr>`;
        tabelaHistorico.innerHTML += linha;
      });
      if (leituras.length > 0) historicoBox.style.display = "block";
    }

    function exportarCSV() {
      const leituras = JSON.parse(localStorage.getItem("leituras") || "[]");
      let csv = "RA,Livro,Data,Acertos\n";
      leituras.forEach(l => {
        csv += `${l.ra},${l.livro},${l.data},${l.acertos}\n`;
      });
      const blob = new Blob([csv], { type: 'text/csv' });
      const link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.download = "historico_leituras.csv";
      link.click();
    }

    form.addEventListener("submit", function(e) {
      e.preventDefault();

      const ra = document.getElementById("ra").value;
      const senha = document.getElementById("senha").value;
      const livro = document.getElementById("livro").value;
      const data = new Date().toLocaleString();
      const acertos = "100%";

      const mensagem = `RA: ${ra}<br>Livro: <strong>${livro}</strong><br>Leitura concluída.<br>Atividades respondidas: <strong>${acertos}</strong><br>Data: ${data}`;

      resultadoBox.innerHTML = mensagem;
      resultadoBox.style.display = "block";

      const novaLeitura = { ra, livro, data, acertos };
      const leituras = JSON.parse(localStorage.getItem("leituras") || "[]");
      leituras.push(novaLeitura);
      localStorage.setItem("leituras", JSON.stringify(leituras));

      atualizarHistorico();
      form.reset();
    });

    atualizarHistorico();
  </script></body>
</html>