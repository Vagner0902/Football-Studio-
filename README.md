# Football-Studio-
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Football Studio - Estratégia de Cartas</title>
  <style>
    body { font-family: Arial; margin: 20px; }
    table, th, td { border: 1px solid #ccc; border-collapse: collapse; padding: 6px; }
    th { background-color: #f2f2f2; }
    input { width: 60px; }
    #suggestion { margin-top: 20px; font-size: 18px; font-weight: bold; }
    .highlight { background-color: #d4ffd4; }
  </style>
</head>
<body>

<h2>Football Studio - Leitura de Cartas</h2>
<table id="roundsTable">
  <thead>
    <tr>
      <th>Rodada</th>
      <th>Carta Home</th>
      <th>Carta Away</th>
      <th>Resultado</th>
      <th>Tipo Home</th>
      <th>Tipo Away</th>
      <th>Tendência Esperada</th>
      <th>Padrão</th>
    </tr>
  </thead>
  <tbody>
    <!-- 10 linhas -->
    <script>
      for (let i = 1; i <= 10; i++) {
        document.write(`
          <tr>
            <td>${i}</td>
            <td><input id="home${i}"></td>
            <td><input id="away${i}"></td>
            <td><input id="result${i}"></td>
            <td id="typeHome${i}"></td>
            <td id="typeAway${i}"></td>
            <td id="expected${i}"></td>
            <td id="pattern${i}"></td>
          </tr>
        `);
      }
    </script>
  </tbody>
</table>

<button onclick="analyze()">Analisar Rodadas</button>

<div id="suggestion"></div>

<script>
  function getCardType(card) {
    card = card.toUpperCase();
    if (["2", "3", "4", "5", "6"].includes(card)) return "Baixo";
    if (["7", "8", "9", "10"].includes(card)) return "Alto";
    if (["J", "Q", "K", "A"].includes(card)) return "Letra";
    return "-";
  }

  function expectedWinner(homeType, awayType) {
    if (homeType === "Alto" && awayType === "Baixo") return "Home";
    if (homeType === "Baixo" && awayType === "Alto") return "Away";
    if (homeType === "Letra" && awayType === "Letra") return "Quebra";
    if (homeType === "Baixo" && awayType === "Baixo") return "Quebra";
    if (homeType === "Letra" && awayType === "Alto") return "Home";
    if (homeType === "Alto" && awayType === "Letra") return "Away";
    if (homeType === "Letra" && awayType === "Baixo") return "Home";
    if (homeType === "Baixo" && awayType === "Letra") return "Away";
    return "-";
  }

  function analyze() {
    let respeita = 0, quebra = 0;
    for (let i = 1; i <= 10; i++) {
      let h = document.getElementById(`home${i}`).value.trim();
      let a = document.getElementById(`away${i}`).value.trim();
      let r = document.getElementById(`result${i}`).value.trim().toUpperCase();

      let th = getCardType(h);
      let ta = getCardType(a);
      let expect = expectedWinner(th, ta);

      let pattern = (r === expect) ? "Respeita" : "Quebra";

      if (expect !== "-" && (r === "HOME" || r === "AWAY"))
        pattern === "Respeita" ? respeita++ : quebra++;

      document.getElementById(`typeHome${i}`).innerText = th;
      document.getElementById(`typeAway${i}`).innerText = ta;
      document.getElementById(`expected${i}`).innerText = expect;
      document.getElementById(`pattern${i}`).innerText = pattern;
    }

    let suggestion = (respeita >= quebra) ? "Seguir Tendência" : "Apostar Contrário";
    document.getElementById("suggestion").innerHTML = `Sugestão: <span class="highlight">${suggestion}</span>`;
  }
</script>

</body>
</html>
