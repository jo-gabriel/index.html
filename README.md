HTML
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculadora Simples </title>
  <link rel="stylesheet"  href="style.css">
</head>
<body>
  <div class="calculadora">
    <!--  Visor da Calculadora  -->
    <input tyoe="text"  id="visor"  disable placeholder="0">
<!-- Botões de Controle e Operações-->
    <div class="botões">
      <button onclick="LimparVisor()"  class="btn-clear">C</button>
      <button onclick="adicionarAoVisor('/')">/</button>
      <button onclick="adicionarAoVisor('*')">*</button>
      <button onclick="adicionarAoVisor('-')">-</button>
      <button onclick="adicionarAoVisor('7')">7</button>
      <button onclick="adicionarAoVisor('8')">8</button>
      <button onclick="adicionarAoVisor('9')">9</button>
      <button onclick="adicionarAoVisor('+')">+</button>
      <button onclick="adicionarAoVisor('4')">4</button>
      <button onclick="adicionarAoVisor('5')">5</button>
      <button onclick="adicionarAoVisor('6')">6</button>
      <button onclick="calcularResultado()" class="btn-equal">=</button>
      <button onclick="adicionarAoVisor('1')">1</button>
      <button onclick="adicionarAoVisor('2')">2</button>
      <button onclick="adicionarAoVisor('3')">3</button>
      <button onclick="adicionarAoVisor('0')">0</button>
    </div>
  </div>
   <script src="script.js"></script>
</body>
</html>
