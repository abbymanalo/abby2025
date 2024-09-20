---
layout: page
title: Whack-A-Mole 
permalink: /mole/
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Whack-a-Mole Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
        }

        #game-board {
            position: relative;
            width: 300px;
            height: 300px;
            margin: 0 auto;
            border: 2px solid #333;
            background-color: #f0f0f0;
        }

        .mole {
            position: absolute;
            width: 50px;
            height: 50px;
            background-color: brown;
            border-radius: 50%;
            display: none;
        }

        #score {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Whac-a-Mole Game</h1>
    <div id="game-board">
        <div class="mole" id="mole"></div>
    </div>
    <div id="score">Score: <span id="score-value">0</span></div>
    <button id="start-button">Start Game</button>
    <script>
        let score = 0;
        let moleInterval;
        const mole = document.getElementById('mole');
        const scoreValue = document.getElementById('score-value');
        const startButton = document.getElementById('start-button');

        startButton.addEventListener('click', startGame);

        function startGame() {
            score = 0;
            scoreValue.textContent = score;
            mole.style.display = 'block';
            moleInterval = setInterval(moveMole, 1000);
        }

        mole.addEventListener('click', () => {
            score++;
            scoreValue.textContent = score;
            moveMole();
        });

        function moveMole() {
            const board = document.getElementById('game-board');
            const x = Math.random() * (board.clientWidth - 50);
            const y = Math.random() * (board.clientHeight - 50);
            mole.style.left = `${x}px`;
            mole.style.top = `${y}px`;
        }
    </script>
</body>
</html>

