<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Mission: Mohamed's 22nd</title>
    <style>
        body { margin: 0; background: #0b0d17; color: white; font-family: 'Courier New', monospace; text-align: center; overflow: hidden; }
        .stars { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1; background: url('https://www.transparenttextures.com/patterns/stardust.png'); }
        .container { padding: 50px; display: none; }
        
        #loader { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #0b0d17; display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 100; }
        .spinner { width: 50px; height: 50px; border: 5px solid rgba(79, 91, 147, 0.3); border-top: 5px solid #4f5b93; border-radius: 50%; animation: spin 1s linear infinite; }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

        #puzzle-board { display: grid; grid-template-columns: repeat(3, 100px); grid-template-rows: repeat(3, 100px); gap: 2px; border: 2px solid #4f5b93; margin: 20px auto; width: 304px; }
        .tile { width: 100px; height: 100px; background-image: url('puzzle.jpg'); background-size: 300px 300px; cursor: pointer; border: 1px solid rgba(255,255,255,0.1); }
        .empty { background: #1a1a2e; cursor: default; }
        #final-message { display: none; animation: fadeIn 2s; }
        button { padding: 10px 15px; background: #4f5b93; color: white; border: none; cursor: pointer; border-radius: 5px; margin: 5px; font-weight: bold; }
        #typewriter-text { max-width: 600px; margin: 40px auto; background: rgba(0,0,0,0.8); padding: 30px; border-radius: 15px; border: 1px solid #4f5b93; line-height: 1.8; min-height: 100px; text-align: left; font-size: 1.1rem; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body onload="startLoading()">
    <div class="stars"></div>

    <div id="loader">
        <div class="spinner"></div>
        <p style="margin-top: 20px; letter-spacing: 5px;">ESTABLISHING SATELLITE LINK...</p>
        <p id="loading-percent">0%</p>
    </div>
    
    <audio id="start-audio"><source src="Interstellar - Miller_s Planet HD(MP3_160K).mp3" type="audio/mpeg"></audio>

    <div class="container" id="game-container">
        <h1>🚀 MISSION CONTROL</h1>
        <p>Satellite Link Secured. Mohamed, begin stabilization.</p>
        <button onclick="initiateMission()" id="init-btn">Start Mission</button>
        <div id="puzzle-board" style="display:none;"></div>
        <p id="move-info" style="display:none;">Moves: <span id="move-count">0</span></p>
    </div>

    <div id="final-message" class="container">
        <h1>✨ ACCESS GRANTED! ✨</h1>
        <div id="typewriter-text"></div>
        <p style="color: #4f5b93; margin-top: 20px;">🛸 End of Transmission.</p>
    </div>

    <script>
        const startAudio = document.getElementById('start-audio');
        let moveCount = 0, firstMoveMade = false;
        let tiles = [0, 1, 2, 3, 4, 5, 6, 7, 8];
        tiles.sort(() => Math.random() - 0.5);

        const message = "Since we don't know each other for a long time, i really don't know what to say XD! But it's a funny thing to do for someone u appreciat being with and it's kinda your speciality so i thought why not? \n\nHappy birthday baladiya guy! Wish u the best in your life, being 22 is not bad u r still young :3 \n\nAnd don't forget! We got some people to kill together XD 🔪🚀";

        function startLoading() {
            let percent = 0;
            const text = document.getElementById('loading-percent');
            const interval = setInterval(() => {
                percent += Math.floor(Math.random() * 10) + 1;
                if (percent >= 100) {
                    percent = 100;
                    clearInterval(interval);
                    setTimeout(() => {
                        document.getElementById('loader').style.display = 'none';
                        document.getElementById('game-container').style.display = 'block';
                        document.body.style.overflow = 'auto';
                    }, 500);
                }
                text.innerText = percent + "%";
            }, 150);
        }

        function initiateMission() {
            document.getElementById('init-btn').style.display = 'none';
            document.getElementById('puzzle-board').style.display = 'grid';
            document.getElementById('move-info').style.display = 'block';
            createBoard();
        }

        function createBoard() {
            const board = document.getElementById('puzzle-board');
            board.innerHTML = '';
            tiles.forEach((tile, index) => {
                const tileDiv = document.createElement('div');
                tileDiv.classList.add('tile');
                if (tile === 8) tileDiv.classList.add('empty');
                else {
                    const row = Math.floor(tile / 3), col = tile % 3;
                    tileDiv.style.backgroundPosition = `-${col * 100}px -${row * 100}px`;
                    tileDiv.onclick = () => moveTile(index);
                }
                board.appendChild(tileDiv);
            });
        }

        function moveTile(index) {
            if (!firstMoveMade) { startAudio.play(); firstMoveMade = true; }
            const emptyIndex = tiles.indexOf(8);
            const row = Math.floor(index / 3), col = index % 3;
            const emptyRow = Math.floor(emptyIndex / 3), emptyCol = emptyIndex % 3;
            if ((Math.abs(row - emptyRow) + Math.abs(col - emptyCol)) === 1) {
                [tiles[index], tiles[emptyIndex]] = [tiles[emptyIndex], tiles[index]];
                moveCount++;
                document.getElementById('move-count').innerText = moveCount;
                createBoard();
                checkWin();
            }
        }

        function checkWin() {
            if (tiles.every((tile, index) => tile === index)) {
                setTimeout(() => {
                    document.getElementById('game-container').style.display = 'none';
                    document.getElementById('final-message').style.display = 'block';
                    typeWriter();
                }, 500);
            }
        }

        let i = 0;
        function typeWriter() {
            if (i < message.length) {
                document.getElementById("typewriter-text").innerHTML += message.charAt(i).replace(/\n/g, '<br>');
                i++;
                setTimeout(typeWriter, 50);
            }
        }
    </script>
</body>
</html>
