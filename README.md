<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Simulador Verde</title>
  <style>
    body {
      background-color: #e0ffe0;
      font-family: Arial, sans-serif;
      padding: 20px;
      color: #006600;
    }

    h2 {
      color: #004d00;
    }

    .caixa {
      border: 2px solid #00cc00;
      padding: 20px;
      background-color: #ccffcc;
      border-radius: 10px;
      max-width: 500px;
      margin: auto;
    }

    input, select, button {
      display: block;
      margin: 10px 0;
      padding: 10px;
      border: 1px solid #00aa00;
      border-radius: 5px;
      background-color: #f0fff0;
      color: #004d00;
    }

    button {
      background-color: #00cc00;
      color: white;
      font-weight: bold;
      cursor: default;
    }
  </style>
</head>
<body>

<div class="caixa">
  <h2>Simulador Verde</h2>
  <label>RA do Aluno:</label>
  <input type="text" disabled placeholder="000000">

  <label>Senha:</label>
  <input type="password" disabled placeholder="••••">

  <label>Série do Aluno:</label>
  <select disabled>
    <option>Selecionado</option>
  </select>

  <label>Livro:</label>
  <input type="text" disabled placeholder="Livro automático">

  <button disabled>Processo Inativo</button>
</div>

</body>
</html>