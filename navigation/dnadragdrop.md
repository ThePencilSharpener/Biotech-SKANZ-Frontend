---
layout: base 
title: DNA Drag and Drop
search_exclude: true
permalink: /dnadraganddrop/
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DNA Island Adventure</title>
    <style>
        canvas {
            border: 1px solid black;
        }
    </style>
</head>
<body>
    <h1>DNA Island Adventure</h1>
    <canvas id="gameCanvas" width="800" height="600"></canvas>
    <div>
        <button onclick="movePlayer('Up')">Up</button>
        <button onclick="movePlayer('Down')">Down</button>
        <button onclick="movePlayer('Left')">Left</button>
        <button onclick="movePlayer('Right')">Right</button>
    </div>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        // Fetch game state and render it
        async function fetchGameState() {
            const response = await fetch('/game_state');
            const gameState = await response.json();
            // Clear canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            // Draw player
            const player = gameState.player_position;
            ctx.fillStyle = 'blue';
            ctx.fillRect(player.x, player.y, 40, 40);
            // Draw scene (mocked for simplicity)
            if (gameState.scene === 'hub') {
                ctx.fillStyle = 'green';
                ctx.fillText('Hub Scene', 10, 20);
            } else if (gameState.scene === 'dna_forest') {
                ctx.fillStyle = 'brown';
                ctx.fillText('Forest Scene', 10, 20);
            }
        }
        // Move player
        async function movePlayer(direction) {
            await fetch('/move_player', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ direction }),
            });
            fetchGameState();
        }
        // Initial render
        fetchGameState();
    </script>
</body>
</html>