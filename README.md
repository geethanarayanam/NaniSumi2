<html lang="te">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Anniversary Nani & Nithya!</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --heart-red: #ff0040;
            --gold: #ffd700;
        }
        body, html { margin: 0; padding: 0; height: 100%; font-family: 'Poppins', sans-serif; overflow: hidden; background: #000; }
        
        /* PAGE TRANSITIONS */
        .page { display: none; height: 100vh; width: 100vw; flex-direction: column; align-items: center; justify-content: center; position: absolute; text-align: center; overflow: hidden; }
        .active { display: flex; animation: fadeIn 0.8s ease-in-out forwards; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* --- PAGE 1: GALAXY + REVOLVING HEART + FALLING HEARTS --- */
        #page1 { background: radial-gradient(circle at bottom, #10002b 0%, #000 100%); cursor: pointer; }
        
        /* Galaxy Stars */
        .star { position: absolute; background: white; border-radius: 50%; opacity: 0.5; animation: twinkle 3s infinite; }
        @keyframes twinkle { 0%, 100% { opacity: 0.3; transform: scale(1); } 50% { opacity: 1; transform: scale(1.2); } }

        /* Falling Hearts */
        .falling-heart {
            position: absolute; color: var(--heart-red); font-size: 20px;
            top: -50px; user-select: none; pointer-events: none;
            animation: fall linear forwards;
        }
        @keyframes fall {
            to { transform: translateY(110vh) rotate(360deg); }
        }

        /* The Big Revolving Heart */
        .heart-box {
            position: relative; width: 200px; height: 200px;
            animation: revolve 6s linear infinite;
            z-index: 10;
        }
        @keyframes revolve {
            0% { transform: rotateY(0deg); }
            100% { transform: rotateY(360deg); }
        }
        .main-heart {
            position: absolute; width: 140px; height: 140px;
            background: var(--heart-red); transform: rotate(45deg);
            box-shadow: 0 0 80px var(--heart-red);
            top: 30px; left: 30px;
        }
        .main-heart::before, .main-heart::after {
            content: ""; position: absolute; width: 140px; height: 140px;
            background: var(--heart-red); border-radius: 50%;
        }
        .main-heart::before { left: -70px; }
        .main-heart::after { top: -70px; }

        .title-text {
            font-family: 'Dancing Script', cursive; font-size: 35px;
            color: white; margin-top: 100px; text-shadow: 0 0 15px var(--heart-red);
            z-index: 10;
        }

        /* --- PAGE 2: GREETING --- */
        #page2 { background: #fff0f3; color: #333; }
        .card { background: white; padding: 40px; border-radius: 20px; box-shadow: 0 15px 40px rgba(0,0,0,0.1); border: 2px solid var(--heart-red); max-width: 80%; }
        
        /* --- PAGE 3: GIFT --- */
        #page3 { background: #0f0f0f; }
        .gift { font-size: 150px; cursor: pointer; animation: bounce 1s infinite alternate; }
        @keyframes bounce { from { transform: scale(1); } to { transform: scale(1.1); } }

        /* --- PAGE 4: TEMPLE & BLESSINGS --- */
        #page4 { background: linear-gradient(to bottom, #ff9900, #ffcc00); }
        .temple { font-size: 140px; margin-bottom: 20px; filter: drop-shadow(0 0 10px white); }
        .bell { font-size: 100px; cursor: pointer; }
        .bell-ring { animation: ring 0.5s infinite alternate; }
        @keyframes ring { from { transform: rotate(-20deg); } to { transform: rotate(20deg); } }
        .blessing-box {
            background: rgba(255,255,255,0.9); padding: 25px; border-radius: 15px;
            color: #800000; font-weight: bold; font-size: 24px; margin-top: 25px;
            display: none; border: 3px solid #b30000; line-height: 1.5;
        }

        .btn { background: var(--heart-red); color: white; border: none; padding: 15px 40px; border-radius: 50px; font-size: 20px; cursor: pointer; margin-top: 20px; font-weight: bold; }
    </style>
</head>
<body onload="init()">

    <div id="page1" class="page active" onclick="nextPage(2)">
        <div id="galaxy-bg"></div>
        <div class="heart-box">
            <div class="main-heart"></div>
        </div>
        <div class="title-text">Nani & Nithya</div>
        <p style="color: white; opacity: 0.7; margin-top: 10px;">Tap the heart to enter...</p>
    </div>

    <div id="page2" class="page">
        <div class="card">
            <h1 style="color: var(--heart-red); font-family: 'Dancing Script'; font-size: 45px;">Happy Anniversary!</h1>
            <p style="font-size: 20px;"><b>Nani & Nithya</b></p>
            <p>May your journey together be filled with infinite love and joy.</p>
            <button class="btn" onclick="nextPage(3)">Open Gift 🎁</button>
        </div>
    </div>

    <div id="page3" class="page">
        <div class="gift" onclick="openGift()">🎁</div>
        <h2 id="gift-status" style="color: white;">A Surprise for You!</h2>
        <button id="gift-btn" class="btn" style="display:none;" onclick="nextPage(4)">Get Blessings 🪔</button>
    </div>

    <div id="page4" class="page">
        <div class="temple">🛕</div>
        <div class="bell" id="templeBell" onclick="ringBell()">🔔</div>
        <p style="font-weight: bold; color: #4e342e;">Ring the bell for blessings!</p>
        <div id="finalWish" class="blessing-box">
            నాని & నిత్య వివాహ వార్షికోత్సవ శుభాకాంక్షలు!<br>
            మీ ఇద్దరూ కలకాలం సుఖసంతోషాలతో వర్ధిల్లాలని మనస్ఫూర్తిగా కోరుకుంటున్నాను. 🙏
        </div>
    </div>

    <script>
        function init() {
            // Create Galaxy
            const bg = document.getElementById('galaxy-bg');
            for(let i=0; i<150; i++) {
                let s = document.createElement('div');
                s.className = 'star';
                s.style.width = s.style.height = Math.random() * 3 + 'px';
                s.style.left = Math.random() * 100 + 'vw';
                s.style.top = Math.random() * 100 + 'vh';
                s.style.animationDelay = Math.random() * 2 + 's';
                bg.appendChild(s);
            }
            // Start Heart Rain
            setInterval(createFallingHeart, 300);
        }

        function createFallingHeart() {
            const h = document.createElement('div');
            h.className = 'falling-heart';
            h.innerHTML = '❤️';
            h.style.left = Math.random() * 100 + 'vw';
            h.style.fontSize = Math.random() * 20 + 10 + 'px';
            h.style.animationDuration = Math.random() * 3 + 2 + 's';
            h.style.opacity = Math.random();
            document.getElementById('page1').appendChild(h);
            setTimeout(() => h.remove(), 5000);
        }

        function nextPage(n) {
            document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
            document.getElementById('page' + n).classList.add('active');
        }

        function openGift() {
            const g = document.querySelector('.gift');
            g.innerHTML = '💖✨';
            g.style.animation = 'none';
            document.getElementById('gift-status').innerText = "Cheers to your Love!";
            document.getElementById('gift-btn').style.display = 'block';
            // Simple confetti
            for(let i=0; i<40; i++) {
                let c = document.createElement('div');
                c.style.position = 'fixed';
                c.style.left = '50%'; c.style.top = '50%';
                c.style.width = '8px'; c.style.height = '8px';
                c.style.background = ['#ff0040', 'gold', 'white'][Math.floor(Math.random()*3)];
                document.body.appendChild(c);
                c.animate([
                    { transform: 'translate(0,0)', opacity: 1 },
                    { transform: `translate(${(Math.random()-0.5)*800}px, ${(Math.random()-0.5)*800}px)`, opacity: 0 }
                ], { duration: 1500, fill: 'forwards' });
                setTimeout(() => c.remove(), 1500);
            }
        }

        function ringBell() {
            const b = document.getElementById('templeBell');
            b.classList.add('bell-ring');
            setTimeout(() => b.classList.remove('bell-ring'), 2000);
            document.getElementById('finalWish').style.display = 'block';
        }
    </script>
</body>
</html>
