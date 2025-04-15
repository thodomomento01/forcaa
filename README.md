<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Jogo da Forca</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin-top: 50px;
    }

    #wordDisplay {
      font-size: 30px;
      letter-spacing: 10px;
      margin-bottom: 20px;
    }

    #letters button {
      margin: 5px;
      padding: 10px;
      font-size: 18px;
      cursor: pointer;
    }

    #letters button:disabled {
      background-color: #ccc;
      cursor: not-allowed;
    }

    #attempts {
      font-size: 20px;
      margin-top: 20px;
    }

    #resetButton {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <h1>Jogo da Forca</h1>
  <div id="wordDisplay">_ _ _ _ _</div>
  <div id="letters"></div>
  <div id="attempts">Tentativas restantes: 6</div>
  <button id="resetButton" onclick="startGame()">Reiniciar Jogo</button>

  <script>
    const palavras = ['banana', 'abacaxi', 'laranja', 'morango', 'uva', 'manga'];
    let palavraSecreta = '';
    let letrasCorretas = [];
    let tentativas = 6;

    const lettersContainer = document.getElementById('letters');
    const wordDisplay = document.getElementById('wordDisplay');
    const attemptsDisplay = document.getElementById('attempts');

    // Função para criar os botões de letras
    function criarBotoes() {
      lettersContainer.innerHTML = '';
      for (let i = 65; i <= 90; i++) {
        const letra = String.fromCharCode(i).toLowerCase();
        const botao = document.createElement('button');
        botao.innerText = letra;
        botao.onclick = () => tentarLetra(letra, botao);
        lettersContainer.appendChild(botao);
      }
    }

    // Função para atualizar a exibição da palavra
    function atualizarPalavra() {
      let display = '';
      for (const letra of palavraSecreta) {
        display += letrasCorretas.includes(letra) ? letra + ' ' : '_ ';
      }
      wordDisplay.textContent = display.trim();

      // Verifica se o jogador acertou todas as letras
      if (!display.includes('_')) {
        alert('Parabéns! Você venceu!');
        desativarBotoes();
      }
    }

    // Função para tentar uma letra
    function tentarLetra(letra, botao) {
      botao.disabled = true;
      if (palavraSecreta.includes(letra)) {
        letrasCorretas.push(letra);
        atualizarPalavra();
      } else {
        tentativas--;
        attemptsDisplay.textContent = `Tentativas restantes: ${tentativas}`;
        if (tentativas === 0) {
          alert(`Você perdeu! A palavra era: ${palavraSecreta}`);
          desativarBotoes();
        }
      }
    }

    // Função para desativar os botões
    function desativarBotoes() {
      const botoes = lettersContainer.querySelectorAll('button');
      botoes.forEach(btn => btn.disabled = true);
    }

    // Função para reiniciar o jogo
    function startGame() {
      palavraSecreta = palavras[Math.floor(Math.random() * palavras.length)];
      letrasCorretas = [];
      tentativas = 6;
      attemptsDisplay.textContent = `Tentativas restantes: ${tentativas}`;
      criarBotoes();
      atualizarPalavra();
    }

    // Inicia o jogo assim que a página é carregada
    startGame();
  </script>

</body>
</html>
