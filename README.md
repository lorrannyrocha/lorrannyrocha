<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Simulador Árvore de Leitura</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    label, select, input { display: block; margin-bottom: 10px; }
    .historico { margin-top: 20px; }
    table, th, td { border: 1px solid #ccc; border-collapse: collapse; padding: 8px; }
  </style>
</head>
<body>

<h2>Simulador Árvore de Leitura</h2>

<form id="formulario">
  <label>RA do Aluno:</label>
  <input type="text" id="ra" required maxlength="12" pattern="[0-9]+">

  <label>Senha:</label>
  <input type="password" id="senha" required minlength="4">

  <label>Série do Aluno:</label>
  <select id="serie" required>
    <option value="">Selecione</option>
    <option value="6º Ano">6º Ano</option>
    <option value="7º Ano">7º Ano</option>
    <option value="8º Ano">8º Ano</option>
    <option value="9º Ano">9º Ano</option>
    <option value="1º Ano EM">1º Ano EM</option>
    <option value="2º Ano EM">2º Ano EM</option>
    <option value="3º Ano EM">3º Ano EM</option>
  </select>

  <button type="submit">Marcar Todos os Livros</button>
</form>

<div class="historico" id="historicoBox">
  <h3>Histórico de Leituras</h3>
  <table>
    <thead>
      <tr><th>RA</th><th>Série</th><th>Livro</th><th>Data</th><th>Acertos</th></tr>
    </thead>
    <tbody id="tabelaHistorico"></tbody>
  </table>
  <button id="btnExportar">Exportar CSV</button>
</div>

<script>
  const historico = [];
  const alunosRegistrados = new Map();
  const listaLivros = [
    "O Pequeno Príncipe",
    "Dom Casmurro",
    "Capitães da Areia",
    "O Alienista",
    "Memórias Póstumas de Brás Cubas",
    "A Hora da Estrela"
  ];

  document.addEventListener("DOMContentLoaded", () => {
    const form = document.getElementById("formulario");
    const btnExportar = document.getElementById("btnExportar");

    form.addEventListener("submit", function (e) {
      e.preventDefault();

      const ra = document.getElementById("ra").value.trim();
      const senha = document.getElementById("senha").value.trim();
      const serie = document.getElementById("serie").value;

      if (!validarRA(ra) || !validarSenha(senha)) {
        alert("RA ou senha inválidos.");
        return;
      }

      if (!alunosRegistrados.has(ra)) {
        alunosRegistrados.set(ra, senha);
      } else if (alunosRegistrados.get(ra) !== senha) {
        alert("Senha incorreta.");
        return;
      }

      // Impede novas execuções se já marcou todos os livros
      const execucoesDoAluno = historico.filter(entry => entry.ra === ocultarRA(ra)).length;
      if (execucoesDoAluno >= listaLivros.length) {
        alert("Todos os livros já foram marcados para este aluno.");
        return;
      }

      listaLivros.forEach(livro => {
        const jaMarcado = historico.find(entry => entry.ra === ocultarRA(ra) && entry.livro === livro);
        if (!jaMarcado) {
          historico.push({
            ra: ocultarRA(ra),
            serie: serie,
            livro: livro,
            data: new Date().toLocaleDateString(),
            acertos: gerarAcertosFixos()
          });
        }
      });

      atualizarTabela();
    });

    btnExportar.addEventListener("click", exportarCSV);
  });

  function validarRA(ra) {
    return /^[0-9]{4,12}$/.test(ra);
  }

  function validarSenha(senha) {
    return senha.length >= 4;
  }

  function ocultarRA(ra) {
    return ra.slice(0, 2) + "***" + ra.slice(-2);
  }

  function gerarAcertosFixos() {
    return 10; // Valor fixo para evitar variações
  }

  function atualizarTabela() {
    const tabela = document.getElementById("tabelaHistorico");
    tabela.innerHTML = "";
    historico.forEach(item => {
      tabela.innerHTML += `<tr>
        <td>${item.ra}</td>
        <td>${item.serie}</td>
        <td>${item.livro}</td>
        <td>${item.data}</td>
        <td>${item.acertos}</td>
      </tr>`;
    });
  }

  function exportarCSV() {
    let csv = "RA,Série,Livro,Data,Acertos\n";
    historico.forEach(item => {
      csv += `${item.ra},${item.serie},${item.livro},${item.data},${item.acertos}\n`;
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