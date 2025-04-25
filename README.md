<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Крестики-нолики</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            margin-bottom: 20px;
        }
        #board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
        }
        .cell {
            width: 100px;
            height: 100px;
            background-color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            cursor: pointer;
            border: 2px solid #333;
            transition: background-color 0.3s;
        }
        .cell:hover {
            background-color: #eaeaea;
        }
        #message {
            margin-top: 20px;
            font-size: 1.5em;
        }
        button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 1em;
        }
    </style>
</head>
<body>

    <h1>Крестики-нолики</h1>
    <div id="board"></div>
    <div id="message"></div>
    <button onclick="resetGame()">Сбросить игру</button>

    <script>
        const board = document.getElementById('board');
        const message = document.getElementById('message');
        let turn = 'X';
        let gameState = ['', '', '', '', '', '', '', '', ''];
        let isGameActive = true;

        const winningConditions = [
            [0, 1, 2],
            [3, 4, 5],
            [6, 7, 8],
            [0, 3, 6],
            [1, 4, 7],
            [2, 5, 8],
            [0, 4, 8],
            [2, 4, 6]
        ];

        function createBoard() {
            gameState.fill('');
            isGameActive = true;
            message.innerText = '';
            board.innerHTML = '';

            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.setAttribute('data-cell-index', i);
                cell.addEventListener('click', cellClicked);
                board.appendChild(cell);
            }
        }

        function cellClicked(event) {
            const clickedCell = event.target;
            const clickedCellIndex = clickedCell.getAttribute('data-cell-index');

            if (gameState[clickedCellIndex] !== '' || !isGameActive) {
                return;
            }

            gameState[clickedCellIndex] = turn;
            clickedCell.innerText = turn;

            checkResult();
            turn = turn === 'X' ? 'O' : 'X';
        }

        function checkResult() {
            for (let i = 0; i < winningConditions.length; i++) {
                const [a, b, c] = winningConditions[i];
                if (gameState[a] && gameState[a] === gameState[b] && gameState[a] === gameState[c]) {
                    message.innerText = `${gameState[a]} выиграл!`;
                    isGameActive = false;
                    return;
                }
            }

            if (!gameState.includes('')) {
                message.innerText = 'Ничья!';
                isGameActive = false;
            }
        }

        function resetGame() {
            createBoard();
        }

        createBoard();
    </script>

</body>
</html>
