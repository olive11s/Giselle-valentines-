# Giselle-valentines-
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>For Giselle ❤️</title>
<style>
    html, body {
        margin: 0; padding: 0; overflow: hidden;
        font-family: 'Arial', sans-serif; touch-action: none; background: #ffe6f2;
    }
    #gamePhase, #valentinePhase, #finalMessage, #startOverlay {
        width: 100vw; height: 100vh; position: absolute; top: 0; left: 0; display: none;
    }
    #startOverlay {
        background: #ffe6f2; display: flex; flex-direction: column; 
        justify-content: center; align-items: center; z-index: 100;
    }
    #gamePhase { background: linear-gradient(to top, #ff9a9e 0%, #fecfef 100%); }
    
    #basket {
        position: absolute; bottom: 8vh; left: 50%;
        width: 100px; height: 100px; transform: translateX(-50%);
        z-index: 10; font-size: 70px;
        display: flex; justify-content: center; align-items: center;
    }

    .tulip {
        position: absolute; width: 60px; height: 60px;
        font-size: 50px; text-align: center; z-index: 5;
    }

    #timer, #score {
        position: absolute; top: 5vh; font-size: 24px;
        font-weight: bold; color: #fff; text-shadow: 1px 1px 5px rgba(0,0,0,0.3); z-index: 20;
    }
    #timer { left: 5vw; }
    #score { right: 5vw; }

    #valentinePhase { 
        background-color: #ffcce6; display: none; flex-direction: column; 
        align-items: center; padding-top: 5vh;
    }
    
    #noMessage {
        color: #d6336c; font-weight: bold; font-size: 20px;
        height: 60px; text-align: center; padding: 0 20px;
        z-index: 60;
    }

    /* Cracking Effect Overlay */
    #crackOverlay {
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        pointer-events: none; z-index: 1000; opacity: 0;
        background: url('https://i.imgur.com/vH9x7zX.png') center/cover no-repeat;
    }

    /* Screen Shake Animation */
    @keyframes shake {
        0% { transform: translate(1px, 1px) rotate(0deg); }
        20% { transform: translate(-3px, 0px) rotate(-1deg); }
        40% { transform: translate(3px, 2px) rotate(1deg); }
        60% { transform: translate(-3px, 1px) rotate(-1deg); }
        80% { transform: translate(3px, -1px) rotate(1deg); }
        100% { transform: translate(1px, -2px) rotate(0deg); }
    }
    .shake { animation: shake 0.3s; }

    #finalMessage { 
        background: white; display: none; flex-direction: column; 
        justify-content: center; align-items: center; text-align: center;
    }
    .message-card {
        background: #fff0f5; padding: 30px; border-radius: 20px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.1); border: 2px solid #ffb3d1;
        max-width: 85%;
    }

    .btn { 
        padding: 15px 25px; font-size: 20px; border-radius: 50px; 
        border: none; cursor: pointer; position: absolute; 
    }
    #yesBtn { 
        background: #ff4d94; color: white; 
        top: 70%; left: 50%; transform: translate(-50%, -50%);
        box-shadow: 0 5px 15px rgba(255,77,148,0.4); z-index: 40;
    }
    #noBtn { 
        background: #aaa; color: white; 
        top: 25%; left: 50%; transform: translateX(-50%);
        z-index: 50; width: 100px;
    }
    
    .falling-emoji {
        position: absolute; top: -60px; z-index: 99;
        animation: fall linear forwards;
    }
    @keyframes fall { to { transform: translateY(110vh) rotate(360deg); } }
</style>
</head>
<body id="mainBody">

<div id="crackOverlay"></div>

<div id="startOverlay" style="display: flex;">
    <h1 style="color: #ff4d94; font-size: 36px;">Hi Giselle! ❤️</h1>
    <p style="color: #d6336c; font-size: 18px;">Catch the tulips 🌷</p>
    <button onclick="initGame()" style="padding: 20px 40px; font-size: 24px; border-radius: 50px; border: none; background: #ff4d94; color: white; margin-top: 20px;">PLAY 🌷</button>
</div>

<div id="gamePhase">
    <div id="timer">Time: 15</div>
    <div id="score">🌷: 0</div>
    <div id="basket">🧺</div>
</div>

<div id="valentinePhase">
    <div id="noMessage">Wait! I have a question...</div>
    <h2 style="font-size: 26px; color: #ff4d94; margin-top: 10px;">Will you be my Valentine? 💌</h2>
    <button id="yesBtn" class="btn">YES! 💖</button>
    <button id="noBtn" class="btn">No 😢</button>
</div>

<div id="finalMessage">
    <div class="message-card">
        <h1 style="color: #ff4d94;">Yay! 💖</h1>
        <p style="font-size: 20px; color: #555; line-height: 1.6;">
            Giselle, you make every day brighter.<br>
            I'm so lucky to have you.<br><br>
            <strong>Happy Valentine's Day!</strong><br>
            💋💋💋
        </p>
    </div>
</div>

<audio id="music" loop>
    <source src="https://www.bensound.com/bensound-music/bensound-romantic.mp3" type="audio/mpeg">
</audio>

<script>
const basket = document.getElementById('basket');
const scoreEl = document.getElementById('score');
const timerEl = document.getElementById('timer');
const music = document.getElementById('music');
const noBtn = document.getElementById('noBtn');
const yesBtn = document.getElementById('yesBtn');
const noMessage = document.getElementById('noMessage');
const body = document.getElementById('mainBody');
const crackOverlay = document.getElementById('crackOverlay');

let score = 0, timeLeft = 15, gameActive = false, noClickCount = 0, yesScale = 1;

const phrases = [
    "No? Don't press that! 🥺",
    "That hurts my feelings... 💔",
    "Are you sure? 🤨",
    "One more time and no kisses! 💋🚫",
    "You're breaking my heart! 😭",
    "Think about the tulips! 🌷",
    "Wrong button! Try the pink one! 👉"
];

function initGame() {
    document.getElementById('startOverlay').style.display = 'none';
    document.getElementById('gamePhase').style.display = 'block';
    music.play().catch(() => {});
    gameActive = true;
    startTimer();
    spawnTulips();
}

function startTimer() {
    const timerInterval = setInterval(() => {
        timeLeft--;
        timerEl.textContent = `Time: ${timeLeft}`;
        if (timeLeft <= 0) {
            clearInterval(timerInterval);
            gameActive = false;
            showValentine();
        }
    }, 1000);
}

function spawnTulips() {
    if (!gameActive) return;
    createTulip();
    setTimeout(spawnTulips, 500);
}

function createTulip() {
    const tulip = document.createElement('div');
    tulip.className = 'tulip';
    tulip.innerHTML = '🌷';
    tulip.style.left = Math.random() * (window.innerWidth - 60) + 'px';
    tulip.style.top = '-60px';
    document.getElementById('gamePhase').appendChild(tulip);
    let pos = -60;
    const fall = setInterval(() => {
        pos += 6;
        tulip.style.top = pos + 'px';
        const bRect = basket.getBoundingClientRect();
        const tRect = tulip.getBoundingClientRect();
        if (tRect.bottom >= bRect.top + 20 && tRect.right >= bRect.left && tRect.left <= bRect.right && tRect.top <= bRect.bottom) {
            score++; scoreEl.textContent = `🌷: ${score}`;
            tulip.remove(); clearInterval(fall);
        }
        if (pos > window.innerHeight) { tulip.remove(); clearInterval(fall); }
    }, 20);
}

document.addEventListener('touchmove', (e) => {
    if (!gameActive) return;
    let touchX = e.touches[0].clientX;
    let newLeft = touchX - (basket.offsetWidth / 2);
    if (newLeft < 0) newLeft = 0;
    if (newLeft > window.innerWidth - basket.offsetWidth) newLeft = window.innerWidth - basket.offsetWidth;
    basket.style.left = newLeft + 'px';
}, { passive: false });

function showValentine() {
    document.getElementById('gamePhase').style.display = 'none';
    document.getElementById('valentinePhase').style.display = 'flex';
}

function handleNoAction(e) {
    if(e) e.preventDefault();
    
    // 1. Shake the screen and show crack
    body.classList.add('shake');
    crackOverlay.style.opacity = '0.6';
    setTimeout(() => {
        body.classList.remove('shake');
        crackOverlay.style.opacity = '0';
    }, 300);

    // 2. Vibrate (if device supports it)
    if (navigator.vibrate) navigator.vibrate(200);

    // 3. Update Message
    noMessage.textContent = phrases[noClickCount % phrases.length];
    noClickCount++;

    // 4. Move "No" button ONLY in TOP half (20% to 50% down)
    const maxX = window.innerWidth - noBtn.offsetWidth;
    const minY = window.innerHeight * 0.2; 
    const maxY = window.innerHeight * 0.5;

    noBtn.style.left = Math.random() * maxX + 'px';
    noBtn.style.top = (Math.random() * (maxY - minY) + minY) + 'px';
    noBtn.style.transform = 'none'; 

    // 5. Grow the "Yes" button (placed in the bottom half)
    yesScale += 0.2;
    yesBtn.style.transform = `translate(-50%, -50%) scale(${yesScale})`;
}

noBtn.addEventListener('touchstart', handleNoAction);
noBtn.addEventListener('click', handleNoAction);

yesBtn.addEventListener('click', () => {
    noMessage.textContent = "I knew you couldn't say no! 😉";
    setTimeout(() => {
        document.getElementById('valentinePhase').style.display = 'none';
        document.getElementById('finalMessage').style.display = 'flex';
        celebrate();
    }, 1200);
});

function celebrate() {
    const emojis = ['🌷', '💖', '✨', '💋', '🌸'];
    for(let i=0; i<60; i++) {
        setTimeout(() => {
            const el = document.createElement('div');
            el.className = 'falling-emoji';
            el.innerHTML = emojis[Math.floor(Math.random() * emojis.length)];
            el.style.left = Math.random() * 100 + 'vw';
            el.style.fontSize = Math.random() * 20 + 30 + 'px';
            el.style.animationDuration = Math.random() * 2 + 2 + 's';
            document.body.appendChild(el);
            setTimeout(() => el.remove(), 4000);
        }, i * 80);
    }
}
</script>
</body>
</html>
