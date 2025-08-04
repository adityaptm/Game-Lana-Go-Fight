<!DOCTYPE html>
<html lang="id">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Game Aurhel Alana</title>
    <style>
      body {
        margin: 0;
        font-family: sans-serif;
        background: #111;
      }

      #game {
        width: 80%;
        height: 400px;
        margin: 20px auto;
        border: 2px solid #333;
        position: relative;
        overflow: hidden;
        background-image: url("gbk.png");
        background-size: cover;
        background-position: center 80%;
        background-repeat: no-repeat;
        border-radius: 10px;
        display: none;
      }

      .dino {
        width: 70px;
        height: 90px;
        position: absolute;
        bottom: 0;
        left: 50px;
      }

      .dino img,
      .bibir img {
        width: 100%;
        height: 100%;
        object-fit: contain;
      }

      .bibir {
        width: 70px;
        height: 70px;
        position: absolute;
        bottom: 0;
        left: 100%;
      }

      .score {
        position: absolute;
        top: 8px;
        left: 8px;
        font-size: 20px;
        color: white;
        text-shadow: 0 0 4px black;
        z-index: 20;
      }

      .game-over {
        display: none;
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.7);
        color: white;
        font-size: 24px;
        text-align: center;
        padding-top: 60px;
        z-index: 10;
      }

      .game-over button {
        margin-top: 20px;
        padding: 12px 24px;
        font-size: 16px;
        border: none;
        background: #00c853;
        color: white;
        cursor: pointer;
        border-radius: 5px;
      }

      .volume-control {
        position: absolute;
        top: 8px;
        right: 8px;
        font-size: 12px;
        background: rgba(255, 255, 255, 0.85);
        padding: 4px 8px;
        border-radius: 6px;
        z-index: 20;
      }

      .start-overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.6);
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 30;
      }

      .start-overlay button {
        padding: 16px 32px;
        font-size: 20px;
        background: #00c853;
        border: none;
        color: white;
        border-radius: 10px;
        cursor: pointer;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
      }

      .start-overlay button:hover {
        background: #009d3e;
      }

      .shout-skill {
        position: absolute;
        bottom: 20px;
        right: 20px;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        background: radial-gradient(circle at center, #ff5252, #d50000);
        border: 3px solid z#fff;
        z-index: 40;
        cursor: pointer;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        font-size: 20px;
        font-weight: bold;
        transition: opacity 0.2s;
      }

      .shout-skill.cooldown {
        pointer-events: none;
        opacity: 0.5;
      }

      .cooldown-overlay {
        position: absolute;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        background-color: rgba(0, 0, 0, 0.5);
        z-index: 41;
        pointer-events: none;
        top: 0;
        left: 0;
        clip-path: polygon(
          50% 50%,
          50% 0,
          100% 0,
          100% 100%,
          0 100%,
          0 0,
          50% 0
        );
        animation: cooldownAnim 3s linear forwards;
        display: none;
      }
      .skill-button {
        position: absolute;
        bottom: 20px;
        right: 20px;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        border: none;
        background: rgba(225, 74, 74, 0.85);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
        cursor: pointer;
        overflow: hidden;
        z-index: 30;
      }

      .skill-icon {
        width: 100%;
        height: 100%;
        object-fit: cover;
        border-radius: 50%;
      }

      .cooldown-overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.6);
        color: white;
        font-size: 20px;
        display: none;
        justify-content: center;
        align-items: center;
        border-radius: 50%;
      }

      @keyframes cooldownAnim {
        0% {
          clip-path: polygon(
            50% 50%,
            50% 0,
            100% 0,
            100% 0,
            100% 0,
            100% 0,
            50% 0
          );
        }
        100% {
          clip-path: polygon(
            50% 50%,
            50% 0,
            100% 0,
            100% 100%,
            0 100%,
            0 0,
            50% 0
          );
        }
      }
    </style>
  </head>
  <body>
    <div id="startOverlay" class="start-overlay">
      <button onclick="startGameFromButton()">🎮 Mulai Main</button>
    </div>

    <div id="game">
      <div class="dino" id="dino">
        <img id="dinoSprite" src="lana.png" />
      </div>
      <div class="bibir" id="bibir">
        <img src="bola.png" />
      </div>

      <div class="game-over" id="gameOverScreen">
        <div>
          Game Over!<br />Yah Lana Terjatuh!<br />
          Final Score: <span id="finalScore">0</span>
        </div>
        <button onclick="resetGame()">Main Lagi</button>
      </div> 

      <audio id="bgm" src="" loop></audio>

      <div class="score">Score: <span id="score">0</span></div>

      <div class="volume-control">
        🔊
        <input
          type="range"
          id="volumeSlider"
          min="0"
          max="1"
          step="0.01"
          value="1"
        />
      </div>
      <!-- Tombol Skill Teriak -->
      <button id="shoutSkill" class="skill-button">
        <img src="aurel.png" alt="Skill Teriak" class="skill-icon" />
        <div class="cooldown-overlay" id="cooldownOverlay">0</div>
      </button>

      <script>
        const dino = document.getElementById("dino");
        const dinoSprite = document.getElementById("dinoSprite");
        const bibir = document.getElementById("bibir");
        const scoreDisplay = document.getElementById("score");
        const finalScoreDisplay = document.getElementById("finalScore");
        const gameOverScreen = document.getElementById("gameOverScreen");
        const bgm = document.getElementById("bgm");
        const volumeSlider = document.getElementById("volumeSlider");

        const shoutSkill = document.getElementById("shoutSkill");
        const cooldownOverlay = document.getElementById("cooldownOverlay");

        let isJumping = false,
          score = 0,
          gameOver = false;
        let bibirSpeed = 5,
          bibirX = 0;
        let scoreInterval, collisionCheck, animationFrameId, runInterval;
        let isSkillCooldown = false;

        const runFrames = [
          "lana1.png",
          "lana2.png",
          "lana3.png",
          "lana4.png",
          "lana5.png",
          "lana6.png",
        ];
        const jumpFrames = ["lana7.png", "lana8.png"];
        const idleFrame = "lana.png";

        function animateRun() {
          clearInterval(runInterval);
          let i = 0;
          runInterval = setInterval(() => {
            if (!isJumping && !gameOver) {
              dinoSprite.src = runFrames[i];
              i = (i + 1) % runFrames.length;
            }
          }, 80);
        }

        function jump() {
          if (isJumping || gameOver) return;
          isJumping = true;
          clearInterval(runInterval);

          let jumpIdx = 0;
          dinoSprite.src = jumpFrames[jumpIdx];

          dino.style.transition = "bottom 0.35s linear, transform 0.35s linear";
          dino.style.bottom = "260px";
          dino.style.transform = "translateX(30px)";

          setTimeout(() => {
            jumpIdx = 1;
            dinoSprite.src = jumpFrames[jumpIdx];

            dino.style.transition = "bottom 0.4s linear, transform 0.4s linear";
            dino.style.bottom = "0";
            dino.style.transform = "translateX(0px)";

            setTimeout(() => {
              isJumping = false;
              dino.style.transition = "";
              animateRun();
            }, 400);
          }, 350);
        }

        function moveBibir() {
          if (gameOver) return;
          bibirX -= bibirSpeed;
          if (bibirX < -60)
            bibirX = document.getElementById("game").offsetWidth;
          bibir.style.left = bibirX + "px";
          animationFrameId = requestAnimationFrame(moveBibir);
        }

        function startGame() {
          gameOver = false;
          score = 0;

          const isMobile = /Android|iPhone|iPad|iPod/i.test(
            navigator.userAgent
          );
          bibirSpeed = isMobile ? 2.5 : 5;

          scoreDisplay.innerText = score;
          finalScoreDisplay.innerText = 0;
          gameOverScreen.style.display = "none";
          bgm.volume = volumeSlider.value;
          bgm.play().catch(() => {});
          bibirX = document.getElementById("game").offsetWidth;
          dinoSprite.src = idleFrame;
          animateRun();
          moveBibir();

          scoreInterval = setInterval(() => {
            if (!gameOver) {
              score++;
              scoreDisplay.innerText = score;
              if (score % 100 === 0 && bibirSpeed < 12) bibirSpeed += 0.5;
            }
          }, 100);

          collisionCheck = setInterval(() => {
            const dinoBottom = parseInt(
              getComputedStyle(dino).getPropertyValue("bottom")
            );
            const bibirLeft = parseInt(
              getComputedStyle(bibir).getPropertyValue("left")
            );
            if (bibirLeft > 50 && bibirLeft < 110 && dinoBottom <= 60) {
              endGame();
            }
          }, 10);
        }

        function endGame() {
          gameOver = true;
          cancelAnimationFrame(animationFrameId);
          clearInterval(scoreInterval);
          clearInterval(collisionCheck);
          clearInterval(runInterval);
          dinoSprite.src = idleFrame;
          finalScoreDisplay.innerText = score;
          gameOverScreen.style.display = "block";

          try {
            saveToLeaderboard?.("Player", score);
            renderLeaderboard?.();
          } catch (e) {
            console.warn("Leaderboard not available");
          }
        }

        function resetGame() {
          startGame();
        }

        function startGameFromButton() {
          document.getElementById("startOverlay").style.display = "none";
          document.getElementById("game").style.display = "block";
          startGame();
        }

        // ====== Skill Teriak (bola mental + cooldown + suara) ======
        shoutSkill.addEventListener("click", (event) => {
          event.stopPropagation(); // <-- cegah event click "bocor" ke game area

          if (isSkillCooldown || gameOver) return;

          // Mainkan suara teriak
          const shoutAudio = new Audio("teriak.mp3");
          shoutAudio.volume = volumeSlider.value;
          shoutAudio.play();

          // Bola hilang (disembunyikan)
          bibir.style.display = "none";

          // Cooldown dimulai
          let cooldown = 5;
          isSkillCooldown = true;
          cooldownOverlay.style.display = "flex";
          cooldownOverlay.innerText = cooldown;

          // Setelah 300 ms, bola muncul lagi dari kanan layar
          setTimeout(() => {
            bibirX = document.getElementById("game").offsetWidth;
            bibir.style.left = bibirX + "px";
            bibir.style.display = "block";
          }, 300);

          const cdInterval = setInterval(() => {
            cooldown -= 1;
            if (cooldown <= 0) {
              clearInterval(cdInterval);
              cooldownOverlay.style.display = "none";
              isSkillCooldown = false;
            } else {
              cooldownOverlay.innerText = cooldown;
            }
          }, 1000);
        });

        // Event Listener Umum
        document.addEventListener("keydown", (e) => {
          if (e.code === "Space") {
            e.preventDefault();
            jump();
          }
        });

        document.getElementById("game").addEventListener("click", jump);
        document.getElementById("game").addEventListener("touchstart", jump);

        document.addEventListener("click", () => {
          if (bgm.paused) bgm.play().catch(() => {});
        });

        volumeSlider.addEventListener("input", () => {
          bgm.volume = volumeSlider.value;
        });
      </script>
    </div>
  </body>
</html>
