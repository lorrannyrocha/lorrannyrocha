<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Simulador Árvore de Leitura</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    label, select, input { display: block; margin-bottom: 10px; }
    .resultado, .historico { margin-top: 20px; }
    table, th, td { border: 1px solid #ccc; border-collapse: collapse; padding: 8px; }
  </style>
</head>
<body>

<h2>Simulador Árvore de Leitura</h2>

<form id="formulario">
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

<script>
  const historico = [];

  window.addEventListener("DOMContentLoaded", () => {
    const form = document.getElementById("formulario");

    form.addEventListener("submit", function (e) {
      e.preventDefault();

      const ra = document.getElementById("ra").value;
      const livro = document.getElementById("livro").value;

      const execucoesDoAluno = historico.filter(entry => entry.ra === ra).length;
      if (execucoesDoAluno >= 128) {
        alert("Limite de 128 marcações atingido para este aluno.");
        return;
      }

      const novaEntrada = {
        ra: ra,
        livro: livro,
        data: new Date().toLocaleDateString(),
        acertos: Math.floor(Math.random() * 11)
      };

      historico.push(novaEntrada);
      atualizarTabela();
    });
  });

  function atualizarTabela() {
    const tabela = document.getElementById("tabelaHistorico");
    tabela.innerHTML = "";
    historico.forEach(item => {
      const row = `<tr>
        <td>${item.ra}</td>
        <td>${item.livro}</td>
        <td>${item.data}</td>
        <td>${item.acertos}</td>
      </tr>`;
      tabela.innerHTML += row;
    });
  }

  function exportarCSV() {
    let csv = "RA,Livro,Data,Acertos\n";
    historico.forEach(item => {
      csv += `${item.ra},${item.livro},${item.data},${item.acertos}\n`;
    });

    const blob = new Blob([csv], { type: "text/csv" });
    const url = window.URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "historico.csv";
    a.click();
    window.URL.revokeObjectURL(url);
  }
</script>

</body>
</html>