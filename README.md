<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Mission: Mohamed's 22nd</title>
    <style>
        body { margin: 0; background: #0b0d17; color: white; font-family: 'Courier New', monospace; text-align: center; }
        .stars { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1; background: url('https://www.transparenttextures.com/patterns/stardust.png'); }
        .container { padding: 50px; }
        
        #puzzle-board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 2px;
            border: 2px solid #4f5b93;
            margin: 20px auto;
            width: 304px;
        }
        .tile {
            width: 100px;
            height: 100px;
            background-image: url('puzzle.jpg');
            background-size: 300px 300px;
            cursor: pointer;
            border: 1px solid rgba(255,255,255,0.1);
        }
        .empty { background: #1a1a2e; cursor: default; }

        .hint-container { margin-top: 10px; display: none; }
        .hint-img { width: 150px; border: 1px solid #4f5b93; border-radius: 5px; }

        #final-message { display: none; animation: fadeIn 2s; }
        .birthday-photo { width: 250px; border-radius: 10px; margin: 10px; border: 2px solid #fff; }
        button { padding: 10px; background: #4f5b93; color: white; border: none; cursor: pointer; border-radius: 5px; margin-top: 10px; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body>
    <div class="stars"></div>
    
    <div class="container" id="game-container">
        <h1>🚀 MISSION: IMAGE STABILIZATION</h1>
        <p>Mohamed, fix the astronaut visual to unlock the data.</p>
        
        <div id="puzzle-board"></div>
        <button onclick="toggleHint()">Toggle Hint</button>
        
        <div class="hint-container" id="hint-box">
            <p>Target Visual:</p>
            <img src="puzzle.jpg" class="hint-img">
        </div>
    </div>

    <div id="final-message" class="container">
        <h1>✨ ACCESS GRANTED! ✨</h1>
        <img src="photo1.jpg" class="birthday-photo">
        <img src="photo2.jpg" class="birthday-photo">
        <p style="max-width: 600px; margin: 20px auto; background: rgba(0,0,0,0.7); padding: 20px; border-radius: 10px;">
            Since we don't know each other for a long time, i really don't know what to say XD! <br>
            but it's a funny thing to do for someone u appreciat being with and it's kinda your speciality so i thought why not? <br><br>
            Happy birthday <b>baladiya guy</b>! Wish u the best in your life, being 22 is not bad u r still young :3 <br><br>
            And don't forget! we got some people to kill together XD
        </p>
    </div>

    <script>
        const board = document.getElementById('puzzle-board');
        let tiles = [0, 1, 2, 3, 4, 5, 6, 7, 8];
        
        tiles.sort(() => Math.random() - 0.5);

        function createBoard() {
            board.innerHTML = '';
            tiles.forEach((tile, index) => {
                const tileDiv = document.createElement('div');
                tileDiv.classList.add('tile');
                if (tile === 8) {
                    tileDiv.classList.add('empty');
                } else {
                    const row = Math.floor(tile / 3);
                    const col = tile % 3;
                    tileDiv.style.backgroundPosition = `-${col * 100}px -${row * 100}px`;
                    tileDiv.onclick = () => moveTile(index);
                }
                board.appendChild(tileDiv);
            });
        }

        function moveTile(index) {
            const emptyIndex = tiles.indexOf(8);
            const row = Math.floor(index / 3);
            const col = index % 3;
            const emptyRow = Math.floor(emptyIndex / 3);
            const emptyCol = emptyIndex % 3;

            const isAdjacent = (Math.abs(row - emptyRow) + Math.abs(col - emptyCol)) === 1;

            if (isAdjacent) {
                [tiles[index], tiles[emptyIndex]] = [tiles[emptyIndex], tiles[index]];
                createBoard();
                checkWin();
            }
        }

        function toggleHint() {
            const hint = document.getElementById('hint-box');
            hint.style.display = hint.style.display === 'none' ? 'block' : 'none';
        }

        function checkWin() {
            if (tiles.every((tile, index) => tile === index)) {
                setTimeout(() => {
                    document.getElementById('game-container').style.display = 'none';
                    document.getElementById('final-message').style.display = 'block';
                }, 500);
            }
        }

        createBoard();
    </script>
</body>
</html>
