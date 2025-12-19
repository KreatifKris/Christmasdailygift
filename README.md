# Christmasdailygift




<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Christmas Countdown & WhatsApp Wishes 🎄</title>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Mountains+of+Christmas:wght@700&family=Poppins:wght@300;400;600&display=swap');

        :root {
            --red: #d42426;
            --green: #1a472a;
            --gold: #f8b229;
            --white: #ffffff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background: radial-gradient(circle, var(--green), #0d2b1a);
            color: var(--white);
            font-family: 'Poppins', sans-serif;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            overflow-x: hidden;
        }

        h1 {
            font-family: 'Mountains of Christmas', cursive;
            font-size: clamp(2.5rem, 8vw, 4rem);
            color: var(--red);
            text-shadow: 0 0 15px rgba(255, 255, 255, 0.6);
            margin-bottom: 20px;
            text-align: center;
        }

        /* Countdown */
        .countdown-wrapper {
            display: flex;
            gap: 15px;
            margin-bottom: 40px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .time-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(8px);
            border: 2px solid var(--gold);
            border-radius: 15px;
            padding: 15px;
            min-width: 90px;
            text-align: center;
            box-shadow: 0 10px 20px rgba(0,0,0,0.3);
        }

        .time-card span.num {
            display: block;
            font-size: 2.5rem;
            font-weight: 600;
        }

        .time-card span.label {
            font-size: 0.75rem;
            color: var(--gold);
            text-transform: uppercase;
        }

        /* Form Wish Container */
        .wish-container {
            background: rgba(255, 255, 255, 0.95);
            color: #333;
            padding: 30px;
            border-radius: 20px;
            width: 100%;
            max-width: 500px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            text-align: center;
        }

        .wish-container h2 {
            font-family: 'Mountains of Christmas';
            color: var(--red);
            font-size: 2.2rem;
            margin-bottom: 15px;
        }

        input, textarea {
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 10px;
            font-family: 'Poppins', sans-serif;
        }

        .btn-style {
            width: 100%;
            padding: 12px;
            background: var(--red);
            color: white;
            border: none;
            border-radius: 10px;
            font-weight: 600;
            cursor: pointer;
            transition: 0.3s;
            font-size: 1rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn-wa { background: #25d366; }
        .btn-wa:hover { background: #128c7e; }

        /* Snow */
        .snow {
            position: fixed;
            top: -10px;
            color: white;
            pointer-events: none;
            z-index: 9999;
            animation: fall linear forwards;
        }

        @keyframes fall { to { transform: translateY(105vh); } }
    </style>
</head>
<body>

    <h1>Christmas Countdown</h1>

    <div class="countdown-wrapper">
        <div class="time-card"><span id="days" class="num">00</span><span class="label">Hari</span></div>
        <div class="time-card"><span id="hours" class="num">00</span><span class="label">Jam</span></div>
        <div class="time-card"><span id="mins" class="num">00</span><span class="label">Menit</span></div>
        <div class="time-card"><span id="secs" class="num">00</span><span class="label">Detik</span></div>
    </div>

    <div class="wish-container">
        <div id="formSection">
            <h2>Kirim Ucapan 🎄</h2>
            <p style="margin-bottom: 15px; font-size: 0.9rem;">Isi formulir untuk mengirim ucapan Natal.</p>
            <form id="waForm">
                <input type="text" id="nama" placeholder="Nama Anda" required>
                <textarea id="pesan" rows="4" placeholder="Tulis ucapan selamat Natal..." required></textarea>
                <button type="submit" class="btn-style">Buat Ucapan ✨</button>
            </form>
        </div>

        <div id="confirmSection" style="display: none;">
            <div style="font-size: 3.5rem; margin-bottom: 10px;">🎁</div>
            <h3 style="color: var(--red); margin-bottom: 10px;">Ucapan Sudah Siap!</h3>
            <p style="margin-bottom: 20px;">Silakan klik tombol di bawah untuk lanjut ke WhatsApp.</p>
            <button id="btnKirimWA" class="btn-style btn-wa">Kirim Sekarang</button>
            <p onclick="window.location.reload()" style="margin-top: 15px; font-size: 0.8rem; color: #777; cursor: pointer; text-decoration: underline;">Ulangi / Batal</p>
        </div>
    </div>

    <script>
        // --- PENGATURAN NOMOR ---
        const NOMOR_WA = "6285785218016"; 
        let urlWhatsApp = "";

        // 1. COUNTDOWN
        function updateTimer() {
            const sekarang = new Date();
            let natal = new Date(`December 25, ${sekarang.getFullYear()} 00:00:00`);
            if (sekarang > natal) natal.setFullYear(natal.getFullYear() + 1);
            
            const selisih = natal - sekarang;
            document.getElementById('days').innerText = Math.floor(selisih / 86400000).toString().padStart(2, '0');
            document.getElementById('hours').innerText = Math.floor((selisih / 3600000) % 24).toString().padStart(2, '0');
            document.getElementById('mins').innerText = Math.floor((selisih / 60000) % 60).toString().padStart(2, '0');
            document.getElementById('secs').innerText = Math.floor((selisih / 1000) % 60).toString().padStart(2, '0');
        }
        setInterval(updateTimer, 1000);
        updateTimer();

        // 2. LOGIKA TOMBOL
        const formNatal = document.getElementById('waForm');
        const sectionForm = document.getElementById('formSection');
        const sectionConfirm = document.getElementById('confirmSection');
        const btnFinalKirim = document.getElementById('btnKirimWA');

        formNatal.addEventListener('submit', function(e) {
            e.preventDefault();
            
            const nama = document.getElementById('nama').value;
            const pesan = document.getElementById('pesan').value;
            
            // Format pesan
            const teks = `Halo! 🎄%0A%0ANama: *${nama}*%0AUcapan: _${pesan}_%0A%0ASelamat Natal! ✨`;
            urlWhatsApp = `https://api.whatsapp.com/send?phone=${NOMOR_WA}&text=${teks}`;
            
            // Ganti tampilan
            sectionForm.style.display = 'none';
            sectionConfirm.style.display = 'block';
        });

        btnFinalKirim.addEventListener('click', function() {
            window.open(urlWhatsApp, '_blank');
        });

        // 3. SNOW EFFECT
        setInterval(() => {
            const snow = document.createElement('div');
            snow.className = 'snow';
            snow.innerHTML = '❄';
            snow.style.left = Math.random() * 100 + 'vw';
            snow.style.fontSize = (Math.random() * 10 + 10) + 'px';
            snow.style.opacity = Math.random();
            snow.style.animationDuration = (Math.random() * 3 + 2) + 's';
            document.body.appendChild(snow);
            setTimeout(() => snow.remove(), 5000);
        }, 200);
    </script>
</body>
