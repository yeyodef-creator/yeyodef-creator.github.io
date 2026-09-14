<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Крестики-нолики</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #bfe8f5;
      font-family: Arial, sans-serif;
    }

    .game {
      text-align: center;
    }

    h1 {
      color: #17465a;
      margin-bottom: 20px;
    }

    .status {
      margin-bottom: 15px;
      font-size: 20px;
      color: #17465a;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 8px;
      margin: 0 auto 20px;
    }

    .cell {
      border: none;
      border-radius: 10px;
      background-color: #ffffff;
      font-size: 48px;
      font-weight: bold;
      color: #17465a;
      cursor: pointer;
      box-shadow: 0 3px 8px rgba(0, 0, 0, 0.15);
      transition: background-color 0.2s, transform 0.2s;
    }

    .cell:hover {
      background-color: #e6f8fc;
      transform: scale(1.03);
    }

    .cell:disabled {
      cursor: default;
    }

    .new-game {
      padding: 12px 25px;
      border: none;
      border-radius: 8px;
      background-color: #287c99;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.2s;
    }

    .new-game:hover {
      background-color: #1e6178;
    }
  </style>
</head>
<body>

  <main class="game">
    <h1>Крестики-нолики</h1>

    <div class="status" id="status">Ходит: X</div>

    <div class="board">
      <button class="cell"></button>
      <button class="cell"></button>
      <button class="cell"></button>
      <button class="cell"></button>
      <button class="cell"></button>
      <button class="cell"></button>
      <button class="cell"></button>
      <button class="cell"></button>
      <button class="cell"></button>
    </div>

    <button class="new-game" id="newGame">Новая игра</button>
  </main>

  <script>
    const cells = document.querySelectorAll(".cell");
    const statusText = document.getElementById("status");
    const newGameButton = document.getElementById("newGame");

    let currentPlayer = "X";
    let gameActive = true;
    let board = ["", "", "", "", "", "", "", ""];

    const winningCombinations = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ];

    function checkWinner() {
      for (const combination of winningCombinations) {
        const [a, b, c] = combination;

        if (
          board[a] &&
          board[a] === board[b] &&
          board[a] === board[c]
        ) {
          return board[a];
        }
      }

      if (!board.includes("")) {
        return "Ничья";
      }

      return null;
    }

    function handleCellClick(event) {
      const cell = event.target;
      const index = [...cells].indexOf(cell);

      if (board[index] !== "" || !gameActive) {
        return;
      }

      board[index] = currentPlayer;
      cell.textContent = currentPlayer;
      cell.disabled = true;

      const result = checkWinner();

      if (result) {
        gameActive = false;

        if (result === "Ничья") {
          statusText.textContent = "Ничья!";
        } else {
          statusText.textContent = `Победил игрок ${result}!`;
        }

        return;
      }

      currentPlayer = currentPlayer === "X" ? "O" : "X";
      statusText.textContent = `Ходит: ${currentPlayer}`;
    }

    function startNewGame() {
      board = ["", "", "", "", "", "", "", "", ""];
      currentPlayer = "X";
      gameActive = true;

      cells.forEach(cell => {
        cell.textContent = "";
        cell.disabled = false;
      });

      statusText.textContent = "Ходит: X";
    }

    cells.forEach(cell => {
      cell.addEventListener("click", handleCellClick);
    });

    newGameButton.addEventListener("click", startNewGame);
  </script>

</body>
</html>
